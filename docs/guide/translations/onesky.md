# OneSky

## Introduction

!!! note "Connect to OneSkyApp"

    https://www.oneskyapp.com

OneSky is an external platform for management of translations. There are other translation platforms which can be used to maintain Symfony based translations as well. 

eZ Commerce offers a download feature which uses the API of OneSky. 

Benefits of OneSky:

- user-friendly interface
- several roles and rights for collaborators
- possibility to upload an image to make to contect clear
- possibility to add comments for other collaborators
- easy way to add new languages
- approval process
- and much more...

OneSkyApp offers the possibility to upload the translation files from the shop and edit the translations there. When the translations process is done and translations are approved, they can be downloaded to the shop and then used.

!!! note

    Pay attention, that [the translation process](translations.md) did not change. There is no real connection between the Symfony translation process and OneSkyApp. Symfony doesn´t care, where you are editing the translations and how they come to the shop. The only important thing is, that there are translation files inside of the project.

    ```
    Resources:
        translations:
            messages.de.php
    ```

    OneSkyApp only offers the possibilty to upload, edit and download the translation files.

For the download process eZ Commerce already offers a prepared command.

``` 
php bin/console onesky:download
```

Customers with several domains and multi-language support can create several projects in OneSkyApp if they want to handle the translations in the shop separately. Then the download command can be also adapted per project.

``` 
php bin/console onesky:download <project_name>
```

## Configuration

To download the translations from OneSky the following configurations are necessary:

``` yaml
parameters:
    one_sky_api_key: <replace with your OneSky API key>
    one_sky_secret: <replace with your OneSky secret key>
    siso_one_sky.projects:
        <project_name>:
            project_id: <project ID from OneSky>
            translations_files_path: <path to your translation folder>
            mappings:
                    - { source_file_name: "messages.en.php", locales: ["en", "de"] }
                    - { source_file_name: "validators.en.php", locales: ["en", "de"] }
```
