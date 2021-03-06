# metalsmith-mapsite

[![npm](https://img.shields.io/npm/v/metalsmith-mapsite.svg)](https://www.npmjs.com/package/metalsmith-mapsite) [![Build Status](https://travis-ci.org/quercy/metalsmith-mapsite.svg)](https://travis-ci.org/quercy/metalsmith-mapsite) [![dependencies Status](https://david-dm.org/quercy/metalsmith-mapsite/status.svg)](https://david-dm.org/quercy/metalsmith-mapsite) [![devDependencies Status](https://david-dm.org/quercy/metalsmith-mapsite/dev-status.svg)](https://david-dm.org/quercy/metalsmith-mapsite?type=dev) [![npm](https://img.shields.io/npm/dm/metalsmith-mapsite.svg)](https://www.npmjs.com/package/metalsmith-mapsite)

> A metalsmith plugin for generating a sitemap

This plugin allows you to generate a [sitemap.xml](http://www.sitemaps.org/protocol.html) from your source files. By default it looks for any `.html` files and processes them with [sitemap.js](https://github.com/ekalinin/sitemap.js).

## Installation

```bash
$ npm install metalsmith-mapsite
```

## CLI Usage

Configuration in `metalsmith.json`:

```json
{
  "plugins": {
    "metalsmith-mapsite": {
      "hostname": "http://www.website.com"
    }
  }
}
```

## JavaScript Usage

```javascript
var sitemap = require('metalsmith-mapsite');

metalsmith(__dirname)
  .use(sitemap('http://www.website.com'))
  .build(function(err){
    if (err) throw err;
  });
```
## Options

You can pass options to `metalsmith-mapsite` with the [Javascript API](https://github.com/segmentio/metalsmith#api) or [CLI](https://github.com/segmentio/metalsmith#cli). The options are:

##### hostname

* `required`

The hostname used for generating the urls.

##### changefreq

* `optional`
* `default: weekly`

Change the default [changefreq](http://www.sitemaps.org/protocol.html).

##### pattern

* `optional`
* `default: '**/*.html'`

A [multimatch](https://github.com/sindresorhus/multimatch) pattern. Only files that match this pattern will be included in the sitemap. Can be a string or an array of strings.

##### priority

* `optional`
* `default: '0.5'`

Change the default [priority](http://www.sitemaps.org/protocol.html).

##### output

* `optional`
* `default: 'sitemap.xml'`

Change the output file for the sitemap.

##### lastmod

* `optional`

Add a lastmodified date to the sitemap. Should be a Date object and can be passed through the Javascript API or the frontmatter.

##### omitExtension

* `optional`
* `default: false`

Will remove extensions from the urls in the sitemap. Useful when you're rewriting urls.

##### omitIndex

* `optional`
* `default: false`

Will replace any paths ending in `index.html` with `''`. Useful when you're using [metalsmith-permalinks](https://github.com/segmentio/metalsmith-permalinks).

##### verbose

* `optional`
* `default: false`

Will output all entries to console.

## Frontmatter

Some values can also be set on a file-to-file basis from a file's frontmatter, the options are:

* `canonical`: will override the filename used to generate the url. The path is relative to the hostname.
* `changefreq`: will override any other settings for `changefreq` for the current file.
* `lastmod`: will override any other settings for `lastmod` for the current file.
* `priority`: will override any other settings for `priority` for the current file.
* `private`: will exclude the file from the sitemap when set to true.
* `sitemap`: include arbitrary structure in sitemap. See [sitemap package](https://github.com/ekalinin/sitemap.js#readme) for examples

For example:

```html
---
canonical: 'different'
changefreq: always
lastmod: 2014-12-01
priority: 1.0
private: true
sitemap:
  img:
    - url: https://resources.example.com/images/image1.png
      caption: Image 1 Caption
      title: Image 1 Title
      geo_location: Australia
      license: https://creativecommons.org/licenses/by/4.0/
    - url: https://resources.example.com/images/image2.png
      caption: Image 2 Caption
      title: Image 2 Title
      geo_location: Australia
      license: https://creativecommons.org/licenses/by/4.0/
---
<!-- index.html -->
```

## Origins

Metalsmith-mapsite is a fork of metalsmith-sitemap. After [refactoring metalsmith-sitemap](https://github.com/ExtraHop/metalsmith-sitemap/pull/12) I decided I would like to keep the plugin [as simple as possible](http://davidwalsh.name/designing-simplicity), to minimize possible points of failure and because it already does a lot.

This conflicted with the maintainer's goals, which is why I forked the plugin to metalsmith-mapsite. Mainly just for personal use, but maybe it'll be of use for others as well!

## License

MIT
