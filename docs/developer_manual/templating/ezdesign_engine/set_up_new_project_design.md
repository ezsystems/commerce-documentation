#  Set up new project design 

1.  Add a new ez\_design.yml to your project bundle.
    Don't remove the base\_theme when adding a project\_theme to the project design\!

    **src/ProjectBundle/Resources/config/ez\_design.yml**

    ``` 
    design_list:
        project_design: [project_theme, base_theme]
    templates_theme_paths:
        project_theme:
            - '%kernel.root_dir%/../src/ProjectBundle/Resources/views'
    ```

2.  Prepend the ez\_design.yml so that the configuration can be used by the ez design engine

    **src/ProjectBundle/DependencyInjection/ProjectExtension.php**

    ``` 
    <?php
    namespace ProjectBundle\DependencyInjection;
    
    use Symfony\Component\DependencyInjection\ContainerBuilder;
    use Symfony\Component\HttpKernel\DependencyInjection\Extension;
    use Symfony\Component\DependencyInjection\Extension\PrependExtensionInterface;
    use Symfony\Component\Yaml\Yaml;
    
    /**
     * This is the class that loads and manages your bundle configuration
     *
     * To learn more see {@link http://symfony.com/doc/current/cookbook/bundles/extension.html}
     */
    class ProjectExtension extends Extension implements PrependExtensionInterface
    {
        /**
         * @param ContainerBuilder $container
         */
        public function prepend( ContainerBuilder $container )
        {
            $config = Yaml::parse(file_get_contents( __DIR__ . '/../Resources/config/ez_design.yml' ));
            $container->prependExtensionConfig( 'ezdesign', $config );
        }
    }
    ```

3.  Add the project\_design either to a SiteAccess or SiteAccess group.

    **app/config/ezplatform.yml**

    ``` 
    # Siteaccess configuration, with one siteaccess per default
    siteaccess:
        list:
          - de
          - en
        groups:
            site_group: [de, en]
        default_siteaccess: en

    site_group:
        design: project_design
    ```

4.  Start developing/overriding templates
