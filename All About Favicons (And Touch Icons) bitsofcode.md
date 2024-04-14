# All About Favicons (And Touch Icons) | bitsofcode
This week I decided to look into the proper ways to use site favicons (and by extension, mobile "touch icons") across browsers. Although there are already quite a few articles on this topic, I decided to write this post as a way for me to bring together everything I have researched and present it in an easy to understand format for myself and, hopefully, you.

The Basics [#](#the-basics)
---------------------------

The favicon (favourite icon) is an image used by browsers to represent a web page. It is typically 16x16 pixels, but varying and larger sizes are now more frequently required due to browsers using them in a growing number of ways -

*   The address bar
*   The links bar
*   Bookmarks
*   Tabs
*   Desktop icon

If we do not specify where to find the favicon, all major browsers (as early as IE5) will by default search for a file named "favicon.ico" in the website's root directory. This means that we technically do not need to declare anything else to have a working favicon.

However, using the ico format can be limiting. It doesn't support transparency, and isn't the most optimized for the web. Nowadays, we are able to use a wider range of formats including [png](https://caniuse.com/#feat=link-icon-png), gif, jpeg, and, in limited circumstances, [svg](https://caniuse.com/#feat=link-icon-svg).

Favicon Declaration & the Link Tag [#](#favicon-declaration-and-the-link-tag)
-----------------------------------------------------------------------------

If deviating from the default, favicons can be declared using the `<link>` tag. The tag accepts the following attributes -

```
<link rel="" type="" sizes="" href="">
```

### Rel [#](#rel)

The rel attribute is used to declare the relationship between the html document and the linked item. We use this for linking stylesheets among other things. With regards to favicons, the [official HTML documentation](https://www.w3.org/html/wg/drafts/html/master/semantics.html#rel-icon) states that this attribute should be set to as -

```
 <link rel="icon">
```

However, as you may have seen before, the attribute is sometimes written as -

```
 <link rel="shortcut icon">
```

This is because some older browsers ([IE8 and below](https://blogs.msdn.com/b/ieinternals/archive/2013/09/08/internet-explorer-favicons-png-link-rel-icon-caching.aspx)) require it to be written this way, and will ignore the entire tag if otherwise. For this reason, although shortcut icon is not formally part of HTML5, it is still recognised and modern browsers will accept it. However, for reasons I will explain, you may only need to use the official method in practice.

### Type [#](#type)

The type attribute specifies the [MIME type format](https://www.iana.org/assignments/media-types/media-types.xhtml#image) of the item being linked. For example an ico file is of the type `image/x-icon` and a png file `image/png`.

[According to the W3C](https://www.w3.org/html/wg/drafts/html/master/semantics.html#attr-link-type), specifying the type is "purely advisory":

> "The type attribute is used as a hint to user agents so that they can avoid fetching resources they do not support... User agents must not consider the type attribute authoritative"

Despite this, IE 9 and 10 require the type attribute to be specified. In these versions, although the need to define the rel attribute as `shortcut icon` was removed, they instead [added a need to specify the media type](https://blogs.msdn.com/b/ieinternals/archive/2011/02/11/ie9-release-candidate-minor-changes-list.aspx) as `image/x-icon`.

Luckily, that is not the case for IE 11, which, like other modern browsers, does not require a media type to be specified.

### Sizes [#](#sizes)

The sizes attribute is used to declare what size you want the particular linked file to be used for. As different sizes are used for different purposes, you can serve optimised files for each of these purposes. This is especially important when using png files, as they are not scalable. Here is a great [cheat sheet](https://github.com/audreyr/favicon-cheat-sheet) which shows what sizes you need and what they are used for.

### Href [#](#href)

This specifies the location of the favicon.

### Putting it together [#](#putting-it-together)

| Browser | Link “rel”/ “type” | Accepted Formats |
| --- | --- | --- |
| IE 8 and below | link rel=”shortcut icon” | ico |
| IE 9, IE 10 | link rel=”icon” type=”image/x-icon” orlink rel=”shortcut icon” | ico |
| IE 11 | link rel=”icon” | ico, png, gif |
| Chrome | link rel=”icon” | ico, png, gif |
| Firefox | link rel=”icon” | ico, png, gif, [svg*](https://caniuse.com/#feat=link-icon-svg) |
| Safari | link rel=”icon” | ico, png, gif |
| Opera | link rel=”icon” | ico, png, gif |

Based on these differences, it may seem like the best method would be to declare two versions - the png files using the modern method and an ico file using the IE8 method.

**However**, it appears that some modern browsers will choose a linked ico file over a linked png file, regardless of the order they are placed. This means that, if we try to accomodate for the older IE browsers, the modern browsers may be served the wrong format.

Because of this, the best solution may be to only declare the png format files, and let older browsers use the default as a fallback.

```


  
<link rel="icon" href="path/to/favicon-16.png" sizes="16x16" type="image/png">  
<link rel="icon" href="path/to/favicon-32.png" sizes="32x32" type="image/png">  
<link rel="icon" href="path/to/favicon-48.png" sizes="48x48" type="image/png">  
<link rel="icon" href="path/to/favicon-62.png" sizes="62x62" type="image/png">


```

Touch Icons for Mobile Devices [#](#touch-icons-for-mobile-devices)
-------------------------------------------------------------------

Some mobile browsers allow users to bookmark the web page to their home screen. We can provide a special icon to be used in these cases, similar to a native application icon.

The way we define what image to use as this "touch icon" is, of course, dependent on the browser/device.

| Device / Browser | Link “rel” | Sizes |
| --- | --- | --- |
| Apple / Safari | link rel=”apple-touch-icon” orlink rel=”apple-touch-icon-precomposed” | 76x76 - iPad 2 and iPad mini120x120 - iPhone 4s, 5, 6152x152 - iPad (retina)180x180 - iPhone 6 Plus |
| Apple / Opera Coast | link rel=”icon”([Will also accept Safari and Windows formats](https://dev.opera.com/articles/opera-coast/)) | 228x228 |
| Android / Chrome | link rel=”icon”([Will, for a limited time, also accept Safari format](https://developer.chrome.com/multidevice/android/installtohomescreen)) | 192x192 |

For Windows, these icons are defined using the meta tag instead.

For Windows 8 / IE 10 -

```
<meta name="msapplication-TileImage" content="pinned-tile.png">  
<meta name="msapplication-TileColor" content="#009900">
```

For Windows 8.1 / IE 11 -

```
  
<meta name="msapplication-config" content="ieconfig.xml" />

  
<?xml version="1.0" encoding="utf-8"?>  
<browserconfig>  
  <msapplication>  
    <tile>  
      <square70x70logo src="images/smalltile.png"/>  
      <square150x150logo src="images/mediumtile.png"/>  
      <wide310x150logo src="images/widetile.png"/>  
      <square310x310logo src="images/largetile.png"/>  
      <TileColor>#009900</TileColor>  
    </tile>  
  </msapplication>  
</browserconfig>


```

That's (pretty much) it! For such a small part of a site, favicons require way too many parts. For very basic sites, I will probably just stick with using the ico format unless I need something fancier. I just can't wait until SVG favicons become a thing, although we are still a long way away!