# OneSky

[OneSky](https://www.oneskyapp.com) is an external platform for managing translations.

eZ Commerce offers a download feature which uses the API of OneSky. 

Benefits of OneSky:

- user-friendly interface
- several roles and rights for collaborators
- possibility to upload an image to make the context clear
- possibility to add comments for other collaborators
- easy way to add new languages
- approval process

OneSky offers the possibility to upload the translation files from the shop and edit the translations there.
When the translation process is done and translations are approved, they can be downloaded to the shop and then used.

[Symfony translations](translations.md) work the samy way regardless of whether you are using OneSky or not.
The only important thing is that there are translation files inside the project.

OneSkyApp only offers the possibility to upload, edit and download the translation files.

To download files from OneSky, use the following command:

``` bash
php bin/console onesky:download
```

Customers with several domains and multi-language support can create several projects in OneSky
if they want to handle the translations in the shop separately.
Then the download command can be also adapted per project:

``` bash
php bin/console onesky:download <project_name>
```

## Configuration

To download translations from OneSky, configure the following settings:

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
