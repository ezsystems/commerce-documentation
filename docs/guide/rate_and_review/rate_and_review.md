# Rate and review

Rate and review is a component that assigns a star rating (1 to 5 stars), a review or a comment to eZ Commerce products.

#### Customer reviews tab

A new tab called **Customer reviews** appears in every product detail page and the logged-in user can create reviews.

![](../img/rate_and_review_1.png)

The tab has a summary part with the total number of reviews and a percentage for every rating.

![](../img/rate_and_review_2.png)

A text field and stars enable the user to enter the rating and review.

![](../img/rate_and_review_3.png)

![](../img/rate_and_review_4.png "A list of reviews")

### Back Office

You can manage rating and review from the Back Office:

![](../img/rate_and_review_5.png)

## Technical description

Rate and review uses the [FOSCommentBundle](https://github.com/FriendsOfSymfony/FOSCommentBundle) for handling comments.

Every user review is stored in the Comment table.

The `thread_id` fields stores the product SKU.

Field state represents the following states:

``` php
const STATE_VISIBLE = 0;
const STATE_DELETED = 1;
const STATE_SPAM = 2;
const STATE_PENDING = 3;
```

Currently states 0 (visible) and 3 (hidden or pending) are supported.
Comments with state 3 are not taken into consideration.

The star rating is stored in the `rating` field. The numbers range from 1 to 5 and represent the number of stars.

### Controllers and services

The original controller from `FOSCommentBundle` is overridden by `SilverEshopThreadController`.

`RateReviewService` (service ID: `siso_core.rate_review_service`) contains additional logic and some customized features.

### Routing

`FOSCommentBundle` uses a REST controller. The following routing entry is added to `routing.yaml`:

``` yaml
fos_comment_api:
    type: rest
    resource: "@SilversolutionsEshopBundle/Resources/config/rest_routing.yml"
    prefix: /api
    defaults: { _format: html }
```

Because the bundle uses REST, the loading of current reviews might not be immediately visible after the product is loaded.

### Templates

The `EshopBundle/Resources/views/Catalog/product.html.twig` template adds a new tab with ratings and comments to product view.

it is only visible if rate and review is enabled in configuration.

### Configuration

To enable the rate and review function, set `reviews_enabled` to `true`:

``` yaml
siso_core.default.reviews_enabled: true
```
