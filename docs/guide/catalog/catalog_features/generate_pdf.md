# Generate PDF

You can generate a PDF from the product detail page using a tool called `wkhtmltopdf`.
 
!!! note

    For security reasons the PDF for product detail can only display general information
    that would be also visible for anonymous users.
    As a consequence for example customer price cannot be displayed.
    
    This is because `wkhtmltopdf` would need user data to generate user-specific PDFs.
    The user data would have to be attached to the URL and would be visible for everyone.  

!!! note

    Use at least version 0.12.4 of `wkhtmltopdf`, because earlier versions might have a bug.
    If your PDF output is too small, you may need to update your `wkhtmltopdf` to a newer version. 

## Using command line

You can generate a PDF in the command line using the URL of the product detail page:

``` bash
wkhtmltopdf --print-media-type http://myshop.local/Product/1234
path_to_pdf/product-detail.pdf
```

## Using the front page

You can add a link or button for generating a PDF in the search page, product list or product detail page.
The link's URL must contain the GET parameter `?view=print`.

For example:

``` html+twig
<a class="button tiny js-add-to-basket" target="_blank" href="{{ catalogElement.seoUrl }}?view=print">PDF</a>
```

`CatalogController` checks if the get parameter `print` is defined and in that case redirects to the PDF.

``` php
$response = $generatePDFService->generatePDFResponse($request);
if (!empty($response)) {
   return $response;
}
```

This page is open in a new tab of the browser, but to open it there the controller first generates the PDF file
in a temporaty folder that is set in configuration:

``` 
#Temp folder where the PDF generated for the products is going to be stored.
silver_eshop.default.pdf_tmp_folder_path: '/tmp/'
```

The logic for this process is included in a new service.

`EshopBundle/Resources/config/ses_services.xml`:

``` xml
<parameter key="siso_core.generate_pdf_service.class">Silversolutions\Bundle\EshopBundle\Service\GeneratePDFService</parameter>
... 
<service id="siso_core.generate_pdf_service" class="%siso_core.generate_pdf_service.class%">
    <argument type="service" id="ezpublish.config.resolver" />
    <argument type="service" id="silver_catalog.url_service" />
</service>
```

`EshopBundle/Service/GeneratePDFService.php`:

``` php
/**
 * Generates a PDF file from the given content (From the Request $request)
 * Uses "wkhtmltopdf" tool with the option --print-media-type to generate the PDF
 * Stores the file in the configured path with a
 *
 * @param Request $request
 * @return null|string
 *
 */
protected function generatePDF(Request $request)
{
    ...

    try {
        // Create the PDF file in the system
        $command = "wkhtmltopdf --print-media-type $urlPath $newPdfPath";
        /** @var Process $process */
        $process = new Process(sprintf($command));
        $process->setTimeout(3600);
        $process->run();
        $process->stop();

        if (file_exists($newPdfPath)) {
            return $newPdfPath;
        }

    } catch (\Exception $e) {}
}
```

The PDF is generated with the data from the Product detail page, see [example](../../img/generate_pdf.pdf).
