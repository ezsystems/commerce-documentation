#  How to: Change logo 

Please make sure before you start to install the [boilerplate](Extending-the-frontend_23561043.html) first

## How do I change the logo?

1.  Edit Resources/views/pagelayout.html.twig inside app folder.

2.  Search for a class c-page-header\_\_logo and just replace the path.

    ``` 
    <img src="{{ asset("bundles/YOUR_BUNDLE_NAME/img/YOUR_LOGO_FILE.png") }}"alt="Logo"/>
    ```

3.  Copy the new image to web/img (create img folder if its not there already)

4.  The new link to the image inside the template should look like this:

    ``` 
    <img src="/img/YOUR_LOGO_FILE.png" alt="Logo"/>
    ```

If you dont see any changes please remove the cache\!
