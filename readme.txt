=== Plugin Name ===
Contributors: omac
Donate link: https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=B38QAQ2DENKEE&lc=US&item_name=Logan%20Graham&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donate_SM%2egif%3aNonHosted
Tags: Lazy, Loading, Images, Plugin
Requires at least: 2.8.0
Tested up to: 5.1.0
Stable tag: 2.0.5
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

A simple plugin to enable lazy loading for all images using WordPress image functions.

== Description ==

A simple plugin to enable lazy loading for all images using WordPress image functions, or embedded into posts and pages using the WordPress dashboard.

The plugin works by replacing the original image source with a blank transparent gif via datauri. Reducing page load speed, network requests, and saving bandwidth for host and visitors unless they scroll where an image is in view. The plugin makes use of the fantastic [lazysizes](https://github.com/aFarkas/lazysizes) javascript library.

== Installation ==

This section describes how to install the plugin and get it working.

1. Upload the plugin files to the `/wp-content/plugins/wp-lazy-loaded-images` directory, or install the plugin through the WordPress plugins screen directly.
2. Activate the plugin through the 'Plugins' screen in WordPress
3. That's it! It should automatically replace images used inside of posts / pages (where passed through properly in themes), plugins, and more.


== Frequently Asked Questions ==

= How can I change the default placeholder color? =

As of 2.0.0, Images now load as a transparent gif, which should remove the need to add custom image colors, and make it work in more situations automatically.

= Fade in images as they load =

With the new lazy load library, a few helper CSS classes are included by default. The full documentation can be found [here](https://github.com/aFarkas/lazysizes#css-api), but the gist of it [pulled from the github repo] is as follows:

     /* fade image in after load */
     .lazyload,
     .lazyloading {
     	opacity: 0;
     }
     .lazyloaded {
     	opacity: 1;
     	transition: opacity 300ms;
     }

= Fallback =

As of 1.4.0, the plugin also adds support for a `noscript` fallback so the images will appear in browsers that might have Javascript disabled.

To take full advantage of this, the plugin provides a fully automatic solution using the following filter to your `functions.php`, `add_filter( 'lazy_load_enable_fallback', '__return_true' )`. This method will automatically handle all of the below instructions. If you're developing a custom theme you might want to include the following instructions manually, in that case you can use `add_filter( 'lazy_load_enable_noscript', '__return_true' )` to only add the `<noscript>` tags and leave the styles, javascript, and body classes untouched.

**NOTE:** This fallback only works on the images that are replaced inside the body of the pages, and those generated by using `the_post_thumbnail`.

Add the following CSS:

     .no-js .lazyload.lazy-fallback {
          display: none;
     }

Add the following JS inside as close to the opening `body` tag as possible:

     <script type="text/javascript">
        var body = document.getElementsByTagName('body')[0];
        if (body != undefined) {
          body.setAttribute('class', body.getAttribute('class').replace('no-js', ''));
        }
     </script>

And append this snippet to your functions.php:

     add_filter( 'body_class', function ( $classes, $class ) {
        if ( ! in_array( 'no-js', $classes ) ) {
           $classes[] = 'no-js';
        }

        return $classes;
     }, 10, 2 );

== Changelog ==

= 2.0.1 - 2.0.5 =
* Fix appending of HTML & BODY tags from generated content
* Update lazysizes library
* Migrate all attributes previously on image to newly generated one(s)

= 2.0.0 =
* Change lazy-loading library to lazysizes
* Images & placeholder images are now generated as transparent gifs, removing the need for custom placeholder colors

= 1.4.0 =
* Added support for external images (or images that can't be found inside of WP install) as long as height, width, and src attributes are set
* Add option for `noscript` fallback using `lazy_load_enable_fallback` filter. Disabled by default for compatibility. See FAQ for instructions.
* Bugfix to catch errors better

= 1.3.2 =
* Add fix for UTF encoding

= 1.3.0 =
* Change `the_content` parser to a DOM parser to be a bit more accurate when replacing images, and better support for attributes (alt, title, classes)
* Add support to disable lazy loading on a per-image basis by passing `data-no-lazy` on the image you don't want loading inside the post/page content area
* Added a few filters for developers / users

= 1.2.0 =
* Move script enqueue to footer
* Allow filter for custom placeholder color on a per-image basis

= 1.1 =
* Add support for automatically replacing images embedded inside posts and pages

= 1.0 =
* Initial Release