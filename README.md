# hugo-mod-twemoji 📦  [![CI](https://github.com/jakejarvis/hugo-mod-twemoji/workflows/CI/badge.svg)](https://github.com/jakejarvis/hugo-mod-twemoji/actions?query=workflow%3ACI)

![](https://user-images.githubusercontent.com/1703673/78717950-64b01680-78ef-11ea-9010-1055e014abbf.png)

[**Twemoji**](https://twemoji.twitter.com/) (Twitter Emoji) is an open-source library of every Unicode emoji ([all 3,500+ of them!](https://emojipedia.org/twitter/)) uniquely redesigned in both SVG and PNG formats. It also provides a script to swap out system-native emojis for these graphics to achieve a uniform appearance across all browsers and platforms. 🙌

This [Hugo Module](https://gohugo.io/hugo-modules/) can be used to import the Twemoji graphics and scripts locally into your Hugo project, rather than making external calls to Twitter's CDN for each individual icon. 

## 🤖 Usage

Give the [Hugo Modules documentation](https://gohugo.io/hugo-modules/) a read to prepare your project, and then add this module to your Hugo project's `config.toml`:

```toml
[module]
[[module.imports]]
  path = "github.com/jakejarvis/hugo-mod-twemoji"
```

The graphics will be mounted in `static/twemoji/svg` and `static/twemoji/png`, and the [minified script](https://github.com/twitter/twemoji/blob/gh-pages/v/13.0.1/twemoji.min.js) in `static/twemoji/js`.

Before you start, you'll probably want to [add Twitter's recommended CSS](https://github.com/twitter/twemoji#inline-styles) to your stylesheet to make sure the Twemojis match the size and alignment of the surrounding text — otherwise they'll be humongous:

```css
img.emoji {
  height: 1em;
  width: 1em;
  margin: 0 .05em 0 .1em;
  vertical-align: -0.1em;
}
```

### ⚡ Quick Start

For a quick start, an optional [partial template](https://gohugo.io/templates/partials/) is mounted at `layouts/partials/twemoji.html`, which does everything described in the section below for you. Include this somewhere near the bottom of your base template, before `</body>`:

```go
{{ partial "twemoji" . }}
```

### ⚙️ Manual

If you don't use the partial, you'll want to call the `js/twemoji.min.js` asset as [a resource](https://gohugo.io/hugo-pipes/introduction/) somewhere in your template or theme's `<head>`, for example:

```html
{{ $twemoji := resources.Get "js/twemoji.min.js" }}
<script src="{{ $twemoji.Permalink }}"></script>
```

...and then in order to actually swap out the emojis, you need to call the script's `twemoji.parse` method. This is where you can choose between SVGs (recommended) or 72x72 PNGs and tell the script where we've placed the graphics. The [official readme](https://github.com/twitter/twemoji#api) has a _lot_ of information about the API, but a good starting point is this one-liner:

```html
<script>
  twemoji.parse(document.body, {{ dict "base" ("twemoji/" | absURL) "folder" "svg" "ext" ".svg" | jsonify | safeJS }})
</script>
```

Simply change `svg` and `.svg` to `png` and `.png` respectively to use the provided 72x72 PNG icons instead.

After building the site this small script will turn into something like:

```html
<script>
  twemoji.parse(document.body, {"base": "https://hugo-mod-twemoji.netlify.com/twemoji/", "ext": ".svg", "folder": "svg"})
</script>
```

## 🌎 Examples

- [https://jarv.is/](https://jarv.is/) ([Source](https://github.com/jakejarvis/jarv.is))

## 📜 Licenses

Twemoji graphics are licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC-BY-4.0) by Twitter, Inc. and other contributors. Code (both Twitter's and this repository) is released under the [MIT License](http://opensource.org/licenses/MIT).

Refer to the main [Twemoji repository](https://github.com/twitter/twemoji) or [website](https://twemoji.twitter.com/) for more information.
