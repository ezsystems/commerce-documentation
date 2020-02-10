#  Frontend Workflow with SASS 

## Pre-requirements

1.  Before you start please make sure you have Node.js and NPM installed on your system. To install it just go to <https://nodejs.org/en/>, download and follow the instructions.

### Official docu pages

  - <http://symfony.com/doc/current/cookbook/assetic/index.html>
  - <https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md>

## Installing required NPM packages

All commands listed below should be run from a command like

1.  Open your command line tool and install gulp globally by running

    ``` 
    npm install -g gulp
    ```

2.  Go to the "app/Resources/public" folder and run

    ``` 
    npm install
    ```

    It will install all dependencies like gulp-sass, browser sync, etc. If you are interested what will be installed check package.json file

## Working with Gulp and Assetic

Thanks to Gulp and Assetic Bundle your work will be much easier and faster. Your assets will be processed and browser will be reloaded automagically for you. To make this work you need run these commands from the root of your project.

1.  Open you project in the browser (Google Chrome recommended)

2.  Go to the project root from command line and run these commands in a separate tabs/windows

    ``` 
    gulp
    // depends on the env that you are running change the --env flag
    php bin/console assetic:watch --env=dev
    ```

3.  Reload browser window to get connected with browser-sync. You should see a message for a short moment in the top right corner of the browser window that says "Connected to browser sync". From now on you don't need to refresh manually anymore.

### Troubleshooting

1.  When you make a mistake in your Sass code gulp might throw an error. If your changes are not directly visible on the page please check the tab/window with gulp command running and check for errors. Fix the error in your code and re-run gulp command.

If you'll get any permission issues try run the commands with sudo.
