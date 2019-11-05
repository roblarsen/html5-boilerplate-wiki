# The build script

The build script is a tool that optimizes your code for production use on the web.

## Why use it?

Faster page load times and happy end users :)

## What it does

* Combines and minifies javascript (via yui compressor)
* Inlines stylesheets specified using `@import` in your CSS
* Combines and minifies CSS
* Optimizes JPGs and PNGs (with jpegtran & optipng)
* Removes development only code (any remaining console.log files, profiling, test suite)
* Basic to aggressive html minification (via htmlcompressor)
* Autogenerates a cache manifest file (and links from the `html` tag) when you enable a property in the project config file.
* Revises the file names of your assets so that you can use heavy caching (1 year expires).
* Upgrades the .htaccess to use heavier caching
* Updates your HTML to reference these new hyper-optimized CSS + JS files
* Updates your HTML to use the minified jQuery instead of the development version
* Remove unneeded references from HTML (like a root folder favicon)
* Runs your JavaScript through a code quality tool (optional)

<img src="http://html5boilerplate.com/img/chart.png">

## Quick Start Guide

### Requirements

### If you're on Mac or Linux...

You've got all your dependencies pre-installed, likely. You _may_ need a `yum install ant-contrib` or what have you.
On Mac , people using ports can run the following command `port install ant-contrib`. (For `brew` look [here](http://www.calvinfroedge.com/installing-apache-ant-with-homebrew-on-mac-os-x/).)

### If you're on Windows...

* Get the [[Java JDK|http://www.oracle.com/technetwork/java/javase/downloads/index.html]] (JRE isn't enough but can someone define which of the four links is preferred?)
* Get [[WinAnt|http://code.google.com/p/winant/]] and point the installer to `Program Files/Java/jre6/bin/`

### Using the Build Script

* 1) Mac users, open the application "Terminal".  Windows users, open command line interface by doing Start Menu > Run > `cmd.exe`.  However, Windows users, we created a friendly `runbuildscript.bat` file for you if you'd like to avoid the command line and navigate to the build directory within your project. 

For those of you new to terminal or command line, use the change directory command followed by the directory path. for example...

    cd sites/your-site/build/

> Note: To ensure you've navigated to the correct directory, you may want to now check the files within the current directory. Mac users can type "ls" in terminal. Windows users should type "dir" in command line. If the file list returned is what you were expecting, move to step 2. Otherwise, check the directory location in Finder or Windows Explorer and start over.

* 2) Next, simply type:

    ant build

The H5BP build script will begin to run and compress your files.  At the very end you should see "BUILD SUCCESSFUL" followed by the total time it took to build.

* 3) Now, look in your H5BP project folder and see that there is a newly created "publish" directory within your project.  Inside, you will find your minified CSS, JS and along with duplicates of the files from your original directory. This new set of files within "publish" is your production code.  The site should look and function the same in browser as it did before, but only now faster!

> Note: If your new pages do not render in browser the same as they did before your ran the build script, visit the Troubleshooting section below.

### Going Further

There are a few different build options:

    ant build     # minor html optimizations (extra quotes removed). inline script/style minified (default)
    ant buildkit  # all html whitespace retained. inline script/style minified 
    ant basics    # same as build minus the basic html minfication
    ant minify    # same as build plus full html minification
    ant text      # same as build but without image (png/jpg) optimizing

Your build will be added to the `publish/` folder. **BOOM!** you're done.

## Configuration

You can override what folders and files you want to operate on in `project.properties`. All the default configuration is in `default.properties`. 

You might also need to dig into the `build.xml` to make stuff happen. It might look a little scary, but, trust us, it used to be much scarier! ;)

You can now add multiple stylesheets to the project (defined in the `project.properties` file) and they will be concatenated into 1 file, with any default stylesheets defined in the `default.properties` file being added at the end.

Adding new 'pages' (html, php, etc) is handled in the same way and the script should iterate through any files listed here and update the css / javascript references as per the original script

All script files that are added within these two lines: `<!-- scripts concatenated and minified via ant build script-->` and `<!-- end scripts-->` will be concatenated and minimized. If you do not wish a script file to be concatenated and minimized, you should link to it outside of those two lines. 

We have also added the ability to define 'environments' for the build process. The original targets still exist, and run the same functions, however each target now has a prod, test and dev environment too. 


### Here is how the environments work:

* `dev` - Increases build number, cleans and copies the build and optimises any images if the target originally did
* `test` - Runs everything that the original target did, however it does not strip the console.log or profiling parts
* `prod` (default) - Runs everything the original target did

To run it you simply use `ant <target> -Denv=<environment>`

    ant build -Denv=dev

If you don't provide the env variable it will use the production environment


## Configuration Cookbook

The following outlines some basic (and not so basic) tasks that you might need to accomplish when using the build script. 

As always, feel free to pitch in and add your own examples. 

### Wordpress integration

Check out Jay George's wordpress screencast videos: http://www.jaygeorge.co.uk/1-html5-buildscript-wordpress-1/

Using the build script on a Wordpress theme introduces a significant problem: Wordpress themes are installed to a website's `/wp-content/themes/<theme-name>` directory while the build script's output references the minified/concatenated files in a relative URI as `js/scripts-xxxx.min.js`. Therefore, output of the build script will reference files in the wrong location.

To make the build script output files that reference the correct url `/wp-content/themes/<theme-name>/js/scripts-xxxx.js`, the build.xml file must be modified. Edit the 2 regular expressions near lines 599 (for js) and 699 (for css) to include the Wordpress PHP that dynamically references the theme's folder: (the line to add is indented)

`    <replaceregexp match="&lt;!-- scripts concatenated [\d\w\s\W]*?!-- `
    `end ((scripts)|(concatenated and minified scripts))--&gt;" `
    `replace="&lt;script src='`
        `&lt;?php bloginfo('template_url'); ?&gt;/`
    `${dir.js}/scripts-${build.number}.min.js\'&gt;&lt;/script&gt;" flags="m">`

### Basic Tasks

Almost every project that uses the build script will need to add/delete or move files and folders from the build script. In the simplest cases this can be achieved by editing the `project.properties` file included in `build/config/`

#### Moving and renaming folders

For something as simple as changing the name of the CSS directory from "css" to "styles" add the following to the "Directory Structure" section

     # Directory Structure
     #
     # Override any directory paths specific to this project
     #
     dir.css	= styles

Some folks like to park static assets in a separate folder to segregate them out from the rest of the application code. If, for example, you wanted to move all of your static files to a folder named "_assets", your Directory Structure section would look something like this:

     # Directory Structure
     #
     # Override any directory paths specific to this project
     #
     dir.js = _assets/js
     dir.css = _assets/css
     dir.images = _assets/images

#### configure deep nested files

There is a special syntax one could use to include or exclude deep nested files in config.

For example, if you put compass source files inside css/src/ folder. but if you are using a setting like the one below, the build script still includes scss files in the build result.

```
file.exclude = *.scss
```

To solve this issue, you could use the syntax below:

```
file.exclude = **/*.scss
```

#### Support for Modules

The build script by default will concatenate and compress all the Javascript code not in the /js/libs folder and create a mangled file name for the now-combined Javascript file.  This is for cache-busting purposes, i.e. you can configure your web server to cache this file on the clients for efficiency.  This is a really great thing because users won't have to ever download that file again.  But what if you change your code?  If you do, the build script will generate a different mangled file name so users will pick up your latest-greatest work.

So by default, the build script puts most of your code in this single, huge Javascript file.  But what if you are using a lot of Javascript, such as with a client-side framework like Backbone or Knockout?  These frameworks can cause you to write a LOT of Javascript, much of it page or module-specific. 

For example a travel web site written with Backbone.js might have a page to show hotel listings, and have some Javascript like this for the page:

```shell
SrchResultsModel.js  -- data model for search results
SrchResultsRouter.js -- a router-dispatcher for navigation or hash events
SrchResultsView.js   -- render and manage some HTML display
```

This code in turn might represent one panel (the search results) of a larger page, which might include other components such as a panel showing weekly specials, a small survey form, and the overall page "frame".  Each of these elements could very well each have multiple Javascript files.

```shell
SpecialsModel.js        SurveyModel.js          FrameModel.js
SpecialsRouter.js       SurveyRouter.js         FrameRouter.js
SpecialsView.js         SurveyView.js           FrameView.js
```

Maybe for your project it makes sense to tie each of these components' files into one-per-component, leaving you with:

```shell
SrchResults.js     Specials.js     Survey.js    Frame.js
```

That's not bad, but let's say for the sake of argument that these 4 files are only useful on some, but not all of your application's pages.  Across your whole application you may have dozens of files like this, but each page may only need a few of them at a time.  For this kind of project it may not make sense to mash everything together in one mother-of-all-Javascript files.  We need something a little more granular.

Enter the /js/modules directory.  (To be clear, "modules" in this context is just a packaging term--not a true module in the sense that a tool like require.js would define them.)  The intent of the /js/modules directory is to hold files that will be minified by the build script, but not concatenated together, allowing you some additional granularity.  So for our example above I might create a directory structure like this:

```shell
/js/modules/components/Survey.js
/js/modules/components/Specials.js
/js/modules/components/SrchResults.js
/js/modules/Frame.js
```

The organization under /js/modules is purely arbitrary.  When the build script runs, everything works as usual _except_ that files in the new /js/modules directory will be minified but otherwise be left alone.  The /js/modules directory name will be name-mangled for cache-busting purposes for the reasons described above.  By default script.js and style.css get the same treatment by the build script, so this is merely extending the idea to the /js/modules directory.  After running the build script you might see something like this in your publish directory:

```shell
/js/69db2Qrt
```

Ok so now that you have put your module Javascript code in /js/modules, you'll use it as any other Javascript file in your HTML pages:
 
```html
<script defer src="js/modules/sample_module.js"></script>
```

The build script modifies references to the /js/modules directory just like it does for script.js and style.css.  It might look something like this after running build:

```html
<script defer src="js/69db2Qrt/sample_module.js"></script>
```

How (or if) you use the modules feature is up to you.  If you need it, this feature will allow you to have more control over which Javascript code gets bundled into the global script.js file and which code is minified but otherwise left alone.

#### Running JSHint and JSLint

Optionally, you can have the build script run your JavaScript files through JSHint or JSLint to help detect errors or potential problems. This is not necessary for any of the other build options, and build failure when running these targets only means that errors were found during the quality check. To run, simply specify one of the following:

    ant build jshint
    ant build jslint

JSHint and JSLint can both be configured with a variety of options. The defaults can be found in the `default.properties` file, and you can override them in `project.properties`. See [[http://jshint.com/]] and [[http://www.jslint.com/lint.html]] for more information.

Note that these targets are set to exclude any JavaScript files ending in ".min.js", as well as files located within the js/libs directory.

#### Running CSSLint

You can check if your CSS files have any issues by running your files through CSSLint and detecting potential problems (note that CSSLint parser is not perfect and has [bugs](https://github.com/nzakas/parser-lib/issues)). Build failure when running these targets only means that errors were found during the quality check of your stylesheet. To run, use:

    ant build csslint


### Advanced Tasks 

The following require a bit more work, often touching on editing or creating tasks in the build.xml itself


### Controlling the order of JavaScript

If you want to concatenate several JavaScript files and the order is important then you can replace the wildcard based concat with specific file names. It's more work, but hopefully you won't have to tweak  this too often. 

For example, the JavaScript task (around line 445) will go from: 


```xml
    <target name="-js.main.concat" depends="-load-build-info" description="(PRIVATE) Concatenates the JS files in dir.js">
        <echo message="Concatenating Main JS scripts..."/>
        <concat destfile="./${dir.publish}/${dir.js}/scripts-${build.number}.js">
            <fileset dir="./${dir.publish}/">
                <include name="**/${dir.js.main}/*.min.js"/>
                <exclude name="**/${dir.js.mylibs}/*.js"/>
                <exclude name="**/${dir.js.libs}/*.js"/>
            </fileset>      
        </concat>
    </target>
```
to this:

```xml
    <!-- JAVASCRIPT -->
    <target name="-js.main.concat" depends="-load-build-info" description="(PRIVATE) Concatenates the JS files in dir.js">
        <echo message="Concatenating Main JS scripts..."/>
        <concat destfile="./${dir.publish}/${dir.js}/scripts-${build.number}.js">
<!--Add one entry for every file-->
	        <fileset file="./${dir.publish}/${dir.js}/plugins.js" />
      		<fileset file="./${dir.publish}/${dir.js}/script.js" />
        </concat>
    </target>
```

## (new) Inlining @imports

### Splitting style.css

Running the ant command `ant css-split` from the build directory will split the monolithic style.css file that you're familiar with into multiple css files.  It then renames itself to style.css.orig and creates a new style.css that @imports the newly split files.  

This is done via special marker comments (ex: `==|== filename ====`), so if you're merging from a personal version of style.css, make sure to include these markers.

If everything went correctly you should see no difference in the behavior of your pages.  The browser should follow the @import directives and load all your CSS.

### Importing Rules

Browser rules for @imports are very strict.  They must follow any `@charset` rules and come before anything else. Any @imports that follow other blocks in the .css file will be ignored by the browser.  The build script isn't this strict, but if you want to maintain browser compatibility you should adhere to these rules.

[w3.org @import rules](http://www.w3.org/TR/css3-cascade/#import)

### Building with Imports

Now whenever the build script is creating a publish directory for you it will attempt to bring in any @import directives it finds to create a single concatenated and minified css file.  This makes it easier for you to add and remove blocks of functionality from your .css during development, while keeping the benefits of a single http GET when deployed.

  * If you are adding your own @imports you should make the path relative to the .css you are adding the @import to.  
  * @import URLs that start with http: or https: won't be inlined, because it is assumed that these are meant to change independent of the file importing them.  
  * @import URLs that start with http: or https: need to come before any @imports that will be inlined, otherwise they will break (see the strict rules above).
  * You can specify media types after an @import (ex: `@import url('print.styles.css') print;`).  The build script will wrap these in an `@media [type]{[file contents]}` block.

## Some notes

 * The build script makes some assumptions about how your javascript files are sorted. 
   * `/js/libs/` contains files from boilerplate. These are minified but not concatenated with others. Modernizr should be alone in the head. jQuery might be pulled off the CDN, and the pngfix is IE6 only.
   * `/js/mylibs/` has your other javascript libraries and plugins. All files in this directory will be minified (unless they end with `.min.js`) and then concatenated together.
   * plugins.js and script.js in the `/js/` folder are all yours. These will also be minified and concatenated (after) with the mylibs files.
 * When the build script changes your HTML to reference the new minified script (usually named something like `scripts-002.min.js`) it looks for some HTML comments which refer to the beginning and end of the script block. Currently it looks for `<!-- scripts concatenated ` and `<!-- end scripts-->`. If you change or strip these comments, the build script will kinda die. :)
 * It likes your filename extensions to be lowercase. Some things might be skipped if you use a `.JPG` extension, for example.
 * Right now, the build.xml et al all have to be in the `./build/` folder. You can't rename it.
 * so all of mylibs min files go into /libs-xxx.min.js
 * all scripts/*.min.js go into scripts-xxx.min.js
 * and then those get smushed together into scripts-xxx.min.js again.
 * Check out issue [[#165 |https://github.com/h5bp/html5-boilerplate/issues/165]] for everything that's planned for next version.
 * Although not officially supported, it is possible to set the publish directory to a relative path outside the project folder. This is useful for generating a snapshot onto a staging server, for instance. In order to do so, set the publish directory to a path relative to the project: `dir.publish = ../mysite/`. Then tell the htmlcompressor script to put the files back where it found them, instead of defining a specific directory. In build.xml, this line `<mapper type="glob" from="*" to="../${dir.publish}/*"/>` becomes this line `<mapper type="glob" from="*" to="*"/>`.
 * Intermediate stages are stored in a new intermediate folder, and only files that should be published are copied into the publish folder.
 * Files are not deleted at the beginning of every build, and files that have already been processed will not be reprocessed unless the source has changed.
 * Versioned files are referenced by a SHA-1 hash of the content rather than a build number. This means that changing your HTML and rebuilding will not cause your users to redownload the same CSS and Javascript, and a reverted change may cause users to use a copy that was previously downloaded. It may be better to use only part of the hash so the HTTP request is shorter.

## Platforms

It should work fine on Windows, Mac and Linux. We are committed to this working cross-platform.

### Dependencies

On Linux/Mac, the image tasks assume you have jpegtran (part of the libjpeg-progs package [[http://www.ijg.org/ ]]) and optipng ([[http://optipng.sourceforge.net/]]) installed and on your path. The binaries are included for Windows users.

#### On Mac OSX

Using [homebrew](http://mxcl.github.com/homebrew/), install the
following packages:

    brew install libjpeg optipng

Using [MacPorts](http://www.macports.org/), install the following
packages:

    port install jpeg optipng

#### On Ubuntu

Using apt, install the following packages:

    apt-get install libjpeg-progs optipng

## Easy VCS-based deployment

Check out the entire repo incl the `build/` folder onto your server. 

Then go into `build/` and run ant.

Then drop this into the top of your .htaccess:

    RewriteEngine On
    RewriteCond $1 !^yourapp/publish/
    RewriteRule ^(.*)$ yourapp/publish/$1 [L]
    # and change yourapp to your actual checkout folder name

This allows you to serve a subfolder as if its the root of your site. So now to update production you can just to do a `git pull && cd build && ant build` or what have you, whenever you want it updated.


## Troubleshooting

Build Script users - On occasion, your site may not render in browser exactly as it did using the original code.  In this case, you may have poorly constructed JavaScript or CSS in your original directory.  Oftentimes, plugins are to blame.

* Debug in your browser developer tool of choice.
* Check your console tab within the dev tool.  This should tell you on which line you can find the error.
* Once you have successfully debugged the error within your original directory, go back to terminal and rerun the build script.  Refresh the site in your browser. If it looks good, you're ready for the WWW! :) 

---

If you are using Ubuntu Linux and receiving the following error when running ant:

    Unable to locate tools.jar. Expected to find it in /usr/lib/jvm/java-6-openjdk/lib/tools.jar

See [this blog post](http://www.ichilton.co.uk/blog/linux/fixing-the-ubuntu-unable-to-locate-tools-jar-error-447.html) for how to resolve the problem.

---

If you receive the following errors:

      BUILD FAILED
      build.xml:136: The following error occurred while executing this line:
      build.xml:441: concat doesn't support the "overwrite" attribute

This is because versions of Ant prior to 1.8.2 do not support the overwrite="no" attribute.

You will need to either upgrade to a later version of Ant or remove all of the occurrences of overwrite="no" from the build.xml file.


## Ports

There is an effort underway to port things to [[rake]]. Woo!


<img src="http://html5boilerplate.com/img/optim.png">
