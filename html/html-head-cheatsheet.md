## HTML `<head>` 元素指南

此份文档中列出了 HTML 文档中 `<head>` 标签中可以放置的一切东西，包括各个的用法及含义。

### 最小推荐

下面是基本的、最低限度的网站基本标签：

```html
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
<title>Page Title</title>
```

其中的 3 个 `meta` 标签 **必须** 放在 `head` 元素的最前面，其他任何形式的内容都必须放在这些标签的*后面*。

### 元素

```html
<!-- Document Title -->
<title>Page Title</title>

<!-- Base URL to use for all relative URLs contained within the document -->
<base href="https://example.com/page.html">

<!-- External CSS -->
<link rel="stylesheet" href="style.css">

<!-- In-document CSS -->
<style>
  /* ... */
</style>

<!-- JavaScript -->
<script src="srcipt.js"></script>
<noscript>
  <!-- no JS alternative -->
</noscript>
```

### `meta` 标签

```html
<!-- set character encoding for the document -->
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">

<!-- allow web authors to choose what version of Internet Explorer the page should be rendered as, visit https://stackoverflow.com/questions/6771258/what-does-meta-http-equiv-x-ua-compatible-content-ie-edge-do for details -->
<meta http-equiv="X-UA-Compatible" content="ie=edge">

<!-- cause the page to scale down to fit content, visit http://stackmirror.bird.so/page/s12uki0ogtv7 for details -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
<!-- The above 3 meta tags must come first in the head; any other head content must come after these tags -->

<!-- allow control over where resources are loaded from -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self'">
<!-- place as early in the document as possible, because only applies to content below this tag -->

<!-- name of web application, only should be used if the website is used as an app -->
<meta name="application-name" content="Application Name">

<!-- short description of the page, limited to 150 characters -->
<meta name="description" content="A description of the page">
<!-- in some situations, this description is used as part of the snippet shown in the search results -->

control the behavior of search engine crawling and indexing
<meta name="robots" content="index,follow"><!-- All Search Engines -->
<meta name="googlebot" content="index,follow"><!-- Google Specific -->

<!-- tell Google not to show the sitelinks search box -->
<meta name="google" content="nositelinkssearchbox">

<!-- tell Google not to provide a translation for this page -->
<meta name="google" content="notranslate">

<!-- verify ownership for Google Search Console -->
<meta name="google-site-verification" content="verification_token">

<!-- verify ownership for Yandex Webmasters -->
<meta name="yandex-verification" content="verification_token">

<!-- verify ownership for Bing Webmaster center -->
<meta name="msvalidate.01" content="verification_token">

<!-- verify ownership for Alexa Console -->
<meta name="alexaVerifyID" content="verification_token">

<!-- verify ownership for Pinterest Console-->
<meta name="p:domain_verify" content="code from pinterest">

<!-- verify ownership for Norton Safe Web -->
<meta name="norton-safeweb-site-verification" content="norton code">

<!-- used to name software used to build the website(i.e. - WordPress, Dreamweaver) -->
<meta name="generator" content="program">

<!-- short description of your site's subject -->
<meta name="subject" content="your website's subject">

<!-- gives a general age rating based on sites content -->
<meta name="rating" content="General">

<!-- allow control over how referrer information is passed -->
<meta name="referrer" content="no-referrer">

<!-- disable automatic detection and formatting of possible phone numbers -->
<meta name="format-detection" content="telephone=no">

<!-- completely opt out of dns prefetching by setting to 'off' -->
<meta http-equiv="x-dns-prefetch-control" content="off">

<!-- store cookie on the client web browser for client identification -->
<meta http-equiv="set-cookie" content="name=value; expires=data; path=url">

<!-- specify the page to appear in a specific frame -->
<meta http-equiv="Window-Target" content="_value">

<!-- geo tags -->
<meta name="ICBM" content="latitude, longitude">
<meta name="geo.position" content="latitude, longitude">
<meta name="geo.region" content="country[-state]"><!-- country code(ISO 3166-1): mandatory, state code(ISO 3166-2): optional; eg. content="US" / content="US-NY" -->
<meta name="geo.placename" content="city/town"><!-- eg. content="New York City" -->
```

- [Google 可以识别的 `meta` 标签](https://support.google.com/webmasters/answer/79812?hl=en)
- [WHATWG Wike: Meta 扩展](https://wiki.whatwg.org/wiki/MetaExtensions)
- [ICBM - 维基百科](https://en.wikipedia.org/wiki/ICBM_address#Modern_use)
- [地理标记 - 维基百科](https://en.wikipedia.org/wiki/Geotagging#HTML_pages)

### 链接

```html
<!-- point to a CSS stylesheet -->
<link rel="stylesheet" href="style.css">

<!-- help prevent duplicate content issues -->
<link rel="canonical" href="https://example.com/2010/06/9-things-to-do-before-entering-social-media.html">

<!-- used to be included before the icon link, but is deprecated and no longer is used -->
<link rel="shortlink" href="https://example.com/?p=42">

<!-- link to an AMP HTML version of the current document -->
<link rel="amphtml" href="https://example.com/path/to/amp-version.html">

<!-- link to a JSON file that specifies "installation" credentials for web applications -->
<link rel="manifest" href="manifest.json">

<!-- link to the author of the document -->
<link rel="author" href="humans.txt">

<!-- refer to a copyright statement that applies to the links content -->
<link rel="license" href="copyright.html">

<!-- give a reference to a location in your document that may be in another language -->
<link rel="alternate" href="https://es.example.com/" hreflang="es">

<!-- give information about an author of another person -->
<link rel="me" href="https://google.com/profiles/thenextweb" type="text/html">
<link rel="me" href="mailto:name@example.com">
<link rel="me" href="sms:+15035550125">

<!-- link to a document that describes a collection of records, documents, or other materials of historaical interest -->
<link rel="archives" href="https://example.com/archives">

<!-- link to top level resource in an hierarchical structure -->
<link rel="index" href="https://example.com/">

<!-- give a self reference - useful when the document has multiple possible references -->
<link rel="self" type="application/atom+xml" href="https://example.com/atomFeed.php?page=3">

<!-- the first, next, previous, and last documents in a series of documents, respectively -->
<link rel="first" href="https://example.com/atomFeed.php">
<link rel="next" href="https://example.com/atomFeed.php?page=4">
<link rel="prev" href="https://example.com/atomFeed.php?page=2">
<link rel="last" href="https://example.com/atomFeed.php?page=147">

<!-- used when using a 3rd party service to maintain a blog -->
<link rel="EditURI" href="https://example.com/xmlrpc.php?rsd" type="application/rsd+xml" title="RSD">

<!-- form an automated comment when another WordPress blog links to your WordPress blog or post -->
<link rel="pingback" href="https://example.com/xmlrpc.php">

<!-- notify a url when you link to it on your site -->
<link rel="webmention" href="https://example.com/webmention">

<!-- load in an external HTML file into the current HTML file -->
<link rel="import" href="/path/to/component.html">

<!-- open Search -->
<link rel="search" href="/open-search.xml" type="application/opensearchdescription+xml" title="Search Title">

<!-- geeds -->
<link rel="alternate" href="https://feeds.feedburner.com/example" type="application/rss+xml" title="RSS">
<link rel="alternate" href="https://example.com/feed.atom" type="application/atom+xml" title="Atom 0.3">

<!-- prefetching, preloading, prebrowsing -->
<link rel="dns-prefetch" href="//example.com/">
<link rel="preconnect" href="https://www.example.com/">
<link rel="prefetch" href="https://www.example.com/">
<link rel="prerender" href="https://example.com/">
<link rel="preload" href="image.png" as="image">
<!-- more info: https://css-tricks.com/prefetching-preloading-prebrowsing/ -->
```

### 网站图标

```html
<!-- For IE 10 and below -->
<!-- Place favicon.ico in the root directory - no tag necessary -->

<!-- For IE 11, Chrome, Firefox, Safari, Opera -->
<link rel="icon" type="image/png" sizes="16x16" href="/path/to/favicon-16x16.png">
<link rel="icon" type="image/png" sizes="32x32" href="/path/to/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="96x96" href="/path/to/favicon-96x96.png">
```

- [所有关于网站图标的信息](https://bitsofco.de/all-about-favicons-and-touch-icons/)
- [网站图标对照表](https://github.com/audreyr/favicon-cheat-sheet)

### 社交

#### Facebook Open Graph

```html
<meta property="fb:app_id" content="123456789">
<meta property="og:url" content="https://example.com/page.html">
<meta property="og:type" content="website">
<meta property="og:title" content="Content Title">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:description" content="Description Here">
<meta property="og:site_name" content="Site Name">
<meta property="og:locale" content="en_US">
<meta property="article:author" content="">
```

- [Facebook Open Graph 标记](https://developers.facebook.com/docs/sharing/webmasters#markup)
- [Open Graph 协议](http://ogp.me/)

#### Facebook Instant Articles

```html
<meta charset="utf-8">
<meta property="op:markup_version" content="v1.0">

<!-- The URL of the web version of your article -->
<link rel="canonical" href="http://example.com/article.html">

<!-- The style to be used for this article -->
<meta property="fb:article_style" content="myarticlestyl
```

- [Facebook Instant Articles: Creating Articles](https://developers.facebook.com/docs/instant-articles/guides/articlecreate)
- [Instant Articles: Format Reference](https://developers.facebook.com/docs/instant-articles/reference)

#### Twitter Cards

```html
<meta name="twitter:card" content="summary">
<meta name="twitter:site" content="@site_account">
<meta name="twitter:creator" content="@individual_account">
<meta name="twitter:url" content="https://example.com/page.html">
<meta name="twitter:title" content="Content Title">
<meta name="twitter:description" content="Content description less than 200 characters">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

- [Twitter Cards: Getting Started Guide](https://dev.twitter.com/cards/getting-started)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)

#### Google+ / Schema.org

```html
<link href="https://plus.google.com/+YourPage" rel="publisher">
<meta itemprop="name" content="Content Title">
<meta itemprop="description" content="Content description less than 200 characters">
<meta itemprop="image" content="https://example.com/image.jpg">
```

#### Pinterest

Pinterest lets you prevent people from saving things from your website, according to their [help center](https://help.pinterest.com/en/articles/prevent-people-saving-things-pinterest-your-site). The `description` is optional.

```html
<meta name="pinterest" content="nopin" description="Sorry, you can't save from my website!">
```

#### OEmbed

```html
<link rel="alternate" type="application/json+oembed"
  href="http://example.com/services/oembed?url=http%3A%2F%2Fexample.com%2Ffoo%2F&amp;format=json"
  title="oEmbed Profile: JSON">
<link rel="alternate" type="text/xml+oembed"
  href="http://example.com/services/oembed?url=http%3A%2F%2Fexample.com%2Ffoo%2F&amp;format=xml"
  title="oEmbed Profile: XML">
```

- [oEmbed 格式](http://oembed.com/)

### 浏览器/平台

### Apple iOS

```html
<!-- Smart App Banner -->
<meta name="apple-itunes-app" content="app-id=APP_ID,affiliate-data=AFFILIATE_ID,app-argument=SOME_TEXT">

<!-- Disable automatic detection and formatting of possible phone numbers -->
<meta name="format-detection" content="telephone=no">

<!-- Add to Home Screen -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="apple-mobile-web-app-title" content="App Title">

<!-- Touch Icons -->
<!-- In most cases, one 180×180px touch icon in the head is enough -->
<link rel="apple-touch-icon" href="/path/to/apple-touch-icon.png">
<!-- Note: Safari on iOS 7 doesn’t add effects to icons. -->
<!-- Older versions of Safari will not add effects for icon files named with the -precomposed.png suffix. -->

<!-- Startup Image ( Deprecated ) -->
<link rel="apple-touch-startup-image" href="/path/to/startup.png">

<!-- iOS app deep linking -->
<meta name="apple-itunes-app" content="app-id=APP-ID, app-argument=http/url-sample.com">
<link rel="alternate" href="ios-app://APP-ID/http/url-sample.com">
```

- [Apple Meta 标签](https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)

#### Apple Safari

```html
<!-- Pinned Site -->
<link rel="mask-icon" href="/path/to/icon.svg" color="red">
```

#### Google Android

```html
<meta name="theme-color" content="#E64545">

<!-- Add to home screen -->
<meta name="mobile-web-app-capable" content="yes">
<!-- More info: https://developer.chrome.com/multidevice/android/installtohomescreen -->

<!-- Android app deep linking -->
<meta name="google-play-app" content="app-id=package-name">
<link rel="alternate" href="android-app://package-name/http/url-sample.com">
```

#### Google Chrome

```html
<link rel="chrome-webstore-item" href="https://chrome.google.com/webstore/detail/APP_ID">

<!-- Disable translation prompt -->
<meta name="google" content="notranslate">
```

#### Google Chrome Mobile (Android Only)

Since Chrome 31, you can set up your web app to "app mode" like Safari.

```html
<!-- Link to a manifest and define the manifest metadata. -->
<!-- The example of manifest.json could be found in the link below. -->
<link rel="manifest" href="manifest.json">

<!-- Define your web page as a web app -->
<meta name="mobile-web-app-capable" content="yes">

<!-- Homescreen Icon  -->
<link rel="icon" sizes="192x192" href="highres-icon.png">
```

- [Google Developer](https://developer.chrome.com/multidevice/android/installtohomescreen)

#### Microsoft Internet Explorer

```html
<meta http-equiv="x-ua-compatible" content="ie=edge">
<meta name="skype_toolbar" content="skype_toolbar_parser_compatible">

<!-- IE10: Disable link highlighting upon tap (https://blogs.windows.com/buildingapps/2012/11/15/adapting-your-webkit-optimized-site-for-internet-explorer-10/) -->
<meta name="msapplication-tap-highlight" content="no">

<!-- Pinned sites (https://msdn.microsoft.com/en-us/library/dn255024(v=vs.85).aspx) -->
<meta name="application-name" content="Sample Title">
<meta name="msapplication-tooltip" content="A description of what this site does.">
<meta name="msapplication-starturl" content="http://example.com/index.html?pinned=true">
<meta name="msapplication-navbutton-color" content="#FF3300">
<meta name="msapplication-window" content="width=800;height=600">
<meta name="msapplication-task" content="name=Task 1;action-uri=http://host/Page1.html;icon-uri=http://host/icon1.ico">
<meta name="msapplication-task" content="name=Task 2;action-uri=http://microsoft.com/Page2.html;icon-uri=http://host/icon2.ico">
<meta name="msapplication-badge" value="frequency=NUMBER_IN_MINUTES;polling-uri=http://example.com/path/to/file.xml">
<meta name="msapplication-TileColor" content="#FF3300">
<meta name="msapplication-TileImage" content="/path/to/tileimage.jpg">

<meta name="msapplication-config" content="http://example.com/browserconfig.xml">
<meta name="msapplication-notification" content="frequency=60;polling-uri=http://example.com/livetile;polling-uri2=http://example.com/livetile2">
<meta name="msapplication-task-separator" content="1">
```

#### App 链接

```html
<!-- iOS -->
<meta property="al:ios:url" content="applinks://docs">
<meta property="al:ios:app_store_id" content="12345">
<meta property="al:ios:app_name" content="App Links">
<!-- Android -->
<meta property="al:android:url" content="applinks://docs">
<meta property="al:android:app_name" content="App Links">
<meta property="al:android:package" content="org.applinks">
<!-- Web Fallback -->
<meta property="al:web:url" content="http://applinks.org/documentation">
<!-- More info: http://applinks.org/documentation/ -->
```

- [App 链接文档](http://applinks.org/documentation/)

### 浏览器（中国）

#### 360 浏览器

```html
<!-- select rendering engine in order -->
<meta name="renderer" content="webkit|ie-comp|ie-stand">
```

#### QQ 浏览器（手机版）

```html
<!-- Locks the screen into the specified orientation -->
<meta name="x5-orientation" content="landscape/portrait">
<!-- Display this page in fullscreen -->
<meta name="x5-fullscreen" content="true">
<!-- Page will be displayed in "application mode"(fullscreen,etc.) -->
<meta name="x5-page-mode" content="app">
```

#### UC 浏览器（手机版）

```html
<!-- Locks the screen into the specified orientation -->
<meta name="screen-orientation" content="landscape/portrait">
<!-- Display this page in fullscreen -->
<meta name="full-screen" content="yes">
<!-- UC browser will display images even if in "text mode" -->
<meta name="imagemode" content="force">
<!-- Page will be displayed in "application mode"(fullscreen,forbiding gesture, etc.) -->
<meta name="browsermode" content="application">
<!-- Disabled the UC browser's "night mode" in this page -->
<meta name="nightmode" content="disable">
<!-- Simplify the page to reduce data transfer -->
<meta name="layoutmode" content="fitscreen">
<!-- Disable the UC browser's feature of "scaling font up when there are many words in this page" -->
<meta name="wap-font-scale" content="no">
```

- [UC 浏览器文档](http://www.uc.cn/download/UCBrowser_U3_API.doc)

### 注意事项

#### 性能

Moving the `href` attribute to the beginning of an element improves compression when GZIP is enabled, because the `href` attribute is used in `a`, `base` and `link` tags.

Example:

```html
<link href="https://fonts.googleapis.com/css?family=Open+Sans:400,700" rel="stylesheet">
```

### 其他资源

- [HTML 样板文档](https://github.com/h5bp/html5-boilerplate/blob/master/dist/doc/html.md)
- [HTML 样板文档](https://github.com/h5bp/html5-boilerplate/blob/master/dist/doc/extend.md)

### 相关项目

- [Atom HTML Head 片段]()
- [Sublime Text HTML Head 片段](https://github.com/marcobiedermann/sublime-head-snippets)
- [head-it：HEAD 片段的 CLI 接口](https://github.com/hemanth/head-it)
- [vue-head：在 Vue.js 中操作 HEAD 标签的 meta 信息](https://github.com/ktquez/vue-head)
