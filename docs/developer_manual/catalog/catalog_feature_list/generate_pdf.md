#  Generate PDF 

There is a functionality to generate a PDF from the product detail page.

For the creation of the PDF a tool call **"*wkhtmltopdf*"** is used**.** This tool is used in different features of eZ Commerce as well, for example for the invoice generation.

Because of the security reasons the PDF for product detail can only display generall information, that would be also visible for anonymous users. As a consequence for example the customer price can not be displayed.  
The reason is, that the tool (wkhtmltopdf) that generates the PDF, would need user data in order to generate user specific PDF. The user data would have to be attached to the url and would be visible/callable for everyone.  
**Important note:**

Please use at least the version 0.12.4 of wkhtmltopdf since earlier versions might have a bug. In case you will see a pretty small PDF you might have to update your wkhtmltopdf to a newer version. 

## Service and tool to generate the PDF

To generate the PDF is possible to do it via terminal with the URL of the product detail page with this command:

**Terminal**

``` 
wkhtmltopdf --print-media-type http://myshop.local/Product/1234
 path_to_pdf/product-detail.pdf
```

**Steps to generate the PDF in eZ Commerce:**

In the search page, product list or product detail page there is a new link / button to see the product detail page in a PDF format.

The link to the PDF has as a requirement that in the URL of it contains the GET parameter ***"?view=print"***

So to include the link to the PDF the link should look like

**template.html.twig**

``` 
<a class="button tiny js-add-to-basket" target="_blank" href="{{ catalogElement.seoUrl }}?view=print">PDF</a>
```

In the catalogController.php checks if the get parameter *"print"* is defined and in that case it redirects to the PDF\!

``` 
$response = $generatePDFService->generatePDFResponse($request);
if (!empty($response)) {
   return $response;
}
```

This page is open in a new tab of the browser, but to open it there first it generates the PDF file in a *"tmp"* folder that is configured in parameters.yml

**EshopBundle/Resources/config/silver.eshop.yml**

``` 
#Temp folder where the PDF generated for the products is going to be stored.
silver_eshop.default.pdf_tmp_folder_path: '/tmp/'
```

To generate the PDF it uses the tool ***wkhtmltopdf***, for more info check the link: <http://wkhtmltopdf.org/>

The logic for this process is included in a **new service**

**EshopBundle/Resources/config/ses\_services.xml** Expand source 

``` 
<parameter key="siso_core.generate_pdf_service.class">Silversolutions\Bundle\EshopBundle\Service\GeneratePDFService</parameter>
... 
<service id="siso_core.generate_pdf_service" class="%siso_core.generate_pdf_service.class%">
    <argument type="service" id="ezpublish.config.resolver" />
    <argument type="service" id="silver_catalog.url_service" />
</service>
```

**EshopBundle/Service/GeneratePDFService.php** Expand source 

``` 
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

The PDF is generated with the data from the Product detail page, see example: [58c135b392adc.pdf](attachments/23560519/23563990.pdf)

## Attachments:

![](images/icons/bullet_blue.gif) [58c135b392adc.pdf](attachments/23560519/23563990.pdf) (application/pdf)  
