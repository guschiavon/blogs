# Using lazysizes with jekyll-responsive-image

## Objective

Use `jekyll-responsive-image` plugin by Joseph Wynn (Github: wildlyinaccurate) together with Alexander Farkas `lazysizes` (Github: aFarkas) to render lazy loading, responsive images with Jekyll

## Setup:

Install the following dependencies & plugin:

### `rmagick`

This is required to generate the different image sizes automatically.
- Run `brew install imagemagick` on MacOS or follow the [Rmagick installation documentation](https://github.com/rmagick/rmagick)
- Add `gem 'rmagick'` to your `Gemfile`
- Run `bundle install` on Terminal

### `lazysizes`

- Head to [lazysizes](https://github.com/aFarkas/lazysizes) to download the script. Copy the code and paste it in a file called `lazysizes.min.js` and save it in the appropriate folder (i.e. `/js`).
- Add `<script src="/js/lazysizes.min.js" async=""></script>` to your markup (i.e. end of `body` tag)
- That's it for now!

### `jekyll-responsive-image`

- Install jekyll-responsive-image in project by adding `gem ‘jekyll-responsive-image’` to the Gemfile and running `bundle install`
- In your `_config.yml` folder, add the following:
```
plugins:
	- jekyll-responsive-image
```
- Inside the `_includes` folder, create a base image template file, name it `dynamic-image.html` or *anything you want*. The file should contain a basic code like this (you can adjust this template to your needs):
```
{% capture srcset %}
    {% for i in resized %}
        /{{ i.path }} {{ i.width }}w,
    {% endfor %}
{% endcapture %}

<img data-src="/{{ path }}" alt="{{ alt }}" data-srcset="{{ srcset | strip_newlines }}" class="lazyload">
```
The `data-src` will pull the `jekyll-responsive-image` generated images into the tag and make it available to `lazysizes` for lazy loading

_Note 1: you can define any class that you want in the `class` attribute but it *is compulsory to put class `lazyload` for lazysizes to work* in conjunction with jekyll-responsive-image_

_Note 2: you can play around with the template and create multiple versions to suit your needs (more on that later)._

#### Configuration

- Configure the plugin by adding the following code to your `_config.yml` file:
```
responsive_image:
  template: _includes/dynamic-image.html # the name of the template file should match the created template
  base_path: assets # update this to match the path to your image folder (default: assets)
  output_path_format: assets/srcset/%{width}/%{basename} # the destination path for the resized images; configure to suit your project structure
```
- There's a variety of configurations for the plugin (check the [documentation](https://github.com/wildlyinaccurate/jekyll-responsive-image)) but *one to have particularly handy is the `sizes` configuration*, which tells the plugin what image size versions to generate for the `srcset`. Your `_config.yml` will look like this:
```
responsive_image:
   template: _includes/dynamic-image.html
   base_path: assets
   sizes:
    - width: 480  # [Required] How wide the resized image will be.
      quality: 80 # [Optional] Overrides default_quality for this size. The default is 90
    - width: 800
    - width: 1400
      quality: 90
```
- Save it and reload Jekyll

## Implementation

Now, in place of any `<img>` tags, you should instead use any of the following structures:

For a simple static image:
```
{% responsive_image path: assets/my-file.jpg %}
```
Or for more attributes like adding `alt` or `title` to an image, follow this syntax:
```
{% responsive_image path: assets/image.jpg alt: "Lorem ipsum..." title: "Lorem ipsum..." %}
```
Or for greater versatility of use, you can play with Liquid attributes, conditional logic and pull values from frontmatter like so:
```
---
image: /assets/placeholder-1.jpg
image-alt: Chill, it's just a placeholder...
---

{% assign path = page.image %}
{% assign alt = page.image-alt %}

{% responsive_image_block %}
  path: {{ path }}
  alt: {{ alt }}
{% if title %}
  title: {{ title }}
  {% endif %}
{% endresponsive_image_block %}
```
To which the output will be:
```
<img data-src="/assets/placeholder-1.jpg" alt="Chill, it's just a placeholder"
     data-srcset=" /assets/srcset/480/placeholder-1.jpg 480w,
                   /assets/srcset/800/placeholder-1.jpg 800w,
                   /assets/srcset/1400/placeholder-1.jpg 1400w,"
                   class="lazyload">
```
Where the image's size is relative to the `sizes` declared on the `_config.yml` and the `width` is relative to the viewport width. You can fiddle with the template to render images related to the pixel density and all sorts of things...
*IMPORTANT:* Data inside the `responsive_image_block` follows YAML syntax, so indentation is important!

- As for multiple `responsive_image_block` templates, you can override the default from the `_config.yml` by declaring the template to be used, like so:
```
---
image: /assets/placeholder-1.jpg
image-alt: Chill, it's just a placeholder...
---

{% assign path = page.image %}
{% assign alt = page.image-alt %}

{% responsive_image_block %}
  template: another-img-template.html
  path: {{ path }}
  alt: {{ alt }}
{% if title %}
  title: {{ title }}
  {% endif %}
{% endresponsive_image_block %}
```

### Troubleshooting & Comments
Got a better way of mixing these two? Think we can make it even better? There are some more improvements to be had, but that's subject to another post!

