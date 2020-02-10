#  SummaryCheckoutFormService 

SummaryCheckoutFormService is a service that implements the logic for the CheckoutSummary form.This service is assigned to the CheckoutSummary form in the [configuration](http://confluence.ng.silverproducts.de/display/EX/Configuration+for+Checkout+Forms).

This service implements the [CheckoutFormServiceInterface, ](http://confluence.ng.silverproducts.de/display/EX/Interfaces+for+checkout+services)[CheckoutSummaryFormServiceInterface](Interfaces-for-checkout-services_23560644.html)[.](http://confluence.ng.silverproducts.de/display/EX/Interfaces+for+checkout+services)

Namespace

    Siso\Bundle\CheckoutBundle\Service\SummaryCheckoutFormService

Service ID

    siso_checkout.checkout_form.summary 

## Usage

**Example**

``` 
$formService = $this->container->get('siso_checkout.checkout_form.summary');
/** @var BasketService $basketService */
$basketService = $this->container->get('silver_basket.basket_service');
$basket = $basketService->getBasket($request);
  
$form = $this->handleForm($request, $data, $basket);
if ($form->isValid()) {
    if ($form->getViewData()->hasChanged()){
       $formService->storeFormDataInBasket($form->getViewData(), $basket);
    }
}
```

## Comments Limit

In the summary there is a comment field where the user can described some information.

By default the comment box does not have a limit, but it is possible to configure a limit it in:

**CheckoutBundle/Resources/config/checkout.yml**

``` 
 siso_checkout.default.checkout_form_summary_max_length: 30
```

The mapping of the request order should be modified to un-limit the number of characters:

**EshopBundle/Resources/mapping/wc3-nav/xsl/include/request.order.xsl** Expand source 

``` 
        <xsl:if test="$message_type != 'calculate'">
            <Comment_Lines singleElementArrays="Comment_Line">
                <xsl:call-template name="comments" >
                    <xsl:with-param name="separator" />
                    </xsl:call-template>
            </Comment_Lines>  
        </xsl:if> 

    <xsl:template name="comments">
        <xsl:param name="separator" select="'||'" />
        <xsl:param name="text" select="SesExtension/Remark" />
        <xsl:choose>
            <xsl:when test="not(contains($text, $separator))">
                <Comment_Line>
                    <Comment>
                        <xsl:value-of select="normalize-space($text)"/>
                    </Comment>
                </Comment_Line>
            </xsl:when>
            <xsl:otherwise>
                <Comment_Line>
                    <Comment>
                        <xsl:value-of select="normalize-space(substring-before($text, $separator))"/>
                    </Comment>
                </Comment_Line>
                <xsl:call-template name="comments">
                    <xsl:with-param name="text" select="substring-after($text, $separator)"/>
                </xsl:call-template>
            </xsl:otherwise>
        </xsl:choose>
    </xsl:template>
```
