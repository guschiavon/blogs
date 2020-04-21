# Enable site-wide settings using Jekyll and Sass
_This tutorial works really well with [CloudCannon CMS](https://cloudcannon.com)_

## Overview

The scope: One of my clients asked me to implement a structure that allows editors to easily change basic settings on the site, like colors, fonts, logos, etc without having to fiddle with code. The projects are hosted on [**CloudCannon**](https://cloudcannon.com), a CMS for Jekyll which is quite awesome. The objective was to _allow editors using CloudCannon to use the CMS's UI for intuitive changes_ of these settings.

After some analysis I've opted to work with `Sass` instead of `CSS` as it is much more powerful for this set up. With that said, it will still work with plain `CSS` but I believe the code will be much more bulky.

### Before we start...

Please note that this process breakdown **assumes that you are familiar with Sass**; if you are not, I highly recommend that you take a small tutorial on how it works. FreeCodeCamp has this [great tutorial](https://www.youtube.com/watch?v=_a5j7KoflTs) that covers some of the basic functions (don't be scared by the 2hr-lenght: focus on the first half hour for a good insight on how Sass works).

_I'm also a beginner with **Sass** so any input and comment is highly appreciated!_

## Set-up & Folder Structure

1. First, install a `Sass/SCSS` processor on your code editor (Atom, etc.). This will ensure that `scss` files are processed into `css` when you save them.
2. Set up your folder structure as usual; add some markup to your `index.html` file for reference.
3. Create a `style.scss` file and place it on your `styles` folder: this is your **main** `scss` file which Jekyll will render
4. Create a `_sass` folder and include all your **Sass partials**, mixins and all here (the Sass `'@import'` partials)
5. Next, create a `_data` **_folder_** and create a `settings.yml` **_file_** inside of it: this is where you will put all the variable values for Jekyll to output from Liquid tags

## Implementation

#### Configuring your `settings.yml` file
Set up the structure & values of your variables here. Colors, font embed codes, images, etc... Anything you want to have on the CloudCannon user interface. This will allow CloudCannon to display these keys in a user-friendly interface on their Explore view. They have a nice database of keys which are displayed with their own UI, which you can check [here](https://docs.cloudcannon.com/editing/interfaces/inputs/). As an example:
```
basic_settings:
  site-name: My awesome site
  site-logo_image: /assets/images/logo.png
  contact-email: hireme@example.com
colors:
  primary_color: '#479aa1'
  accent_color: '#f5aea8'
  dark_color: '#707070'
  light_color: '#f1f1f1'
fonts:
  header_font_family: "'Megrim', cursive;"
  heading-1-font-size: 3rem
  heading-2-font-size: 2rem
  heading-3-font-size: 2rem
  heading-font_code_block: '<link href="https://fonts.googleapis.com/css?family=Megrim&display=swap" rel="stylesheet">'
  paragraph-font-family: "'Gotu', sans-serif;"
  paragraph-font-size: 1rem
  paragraph-line-height: 1.7
  paragraph-font_code_block: '<link href="https://fonts.googleapis.com/css?family=Gotu&display=swap" rel="stylesheet">'
  light-font: 200
  regular-font: 400
  bold-font: 600
  extra-bold-font: 700
```
Note that some of these keys have _**underscore**_ rather than a _dash_; this syntax allows CloudCannon to read the keys and provide the friendly UI for editors, so definitely [check their documentation](https://docs.cloudcannon.com/editing/interfaces/inputs/) for more detailed information.

#### Configuring the `style.scss` file
This file needs to have empty Front Matter for Jekyll to process it; **do so by adding 2 lines of 3 dashes (`---`) at the top of the file**. So:
```
---
---
// Write your SCSS from here onwards
```
Write your `scss` normally, and where you'd normally place the value for the Sass variables (that is, in your `main.scss` stylesheet), place a Liquid tag to pass the values from the `settings.yml` file. Following this project's example, *in your `main.scss` file*, instead of:
```
$primary_color: '#479aa1';
```
you should set:
```
$primary_color:{{site.data.settings.colors.primary_color}};
```
### Overall, the `main.scss` file should look something like this:
```
---
---
// note the 2 lines of dashes with empty front matter above: that's what tell Jekyll to process this file
// describe your Sass variables BEFORE you add partials that take these values (like universal styles) otherwise Jekyll will throw an error as it cannot parse the information for the partial:

$primary_color: {{site.data.settings.colors.primary_color}};
$secondary_color: {{site.data.settings.colors.secondary_color}};

// write your SCSS from here, for example:

h1 { color: $primary_color; }
h2 { background-color: $secondary_color;}

// or a better alternative, add partials from the _sass folder to render the stylesheet:
@import '_resets';
@import '_typography';
```

#### IMPORTANT: settings for `_config.yml` when using `scss` partials

In case you are using partials, you must add the following code to your `_config.yml` file:

```
sass:
    sass_dir: _sass
```
NB: _This is the path for the directory storing the `scss` partials; you can configure it to suit your project structure._

## Troubleshooting
I have run into errors when trying to populate **_variables located in partials_** with YAML values, so the work around is to have all the Sass variables on your main `SCSS` file and use `@import` for the actual styles. Also, you *cannot* process Sass partials with Jekyll (meaning you cannot have empty frontmatter (`---`) in the partial files). At least it didn't work for me, so if you know more, please leave more comments below!
