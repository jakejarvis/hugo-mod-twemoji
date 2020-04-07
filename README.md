# hugo-mod-twemoji 📦

[**Twemoji**](https://twemoji.twitter.com/) (Twitter Emoji) is an open source library of over 3,000 Unicode emojis in both PNG and SVG formats. It also provides a script to swap out system-native emojis for these graphics to achieve a uniform appearance across all browsers and platforms.

This [Hugo Module](https://gohugo.io/hugo-modules/) can be used to import the Twemoji graphics and scripts locally into your Hugo project, rather than making external calls to Twitter's CDN. 

## Usage

Add the module to your Hugo project's `config.toml`:

```toml
[module]
[[module.imports]]
  path = "github.com/jakejarvis/hugo-mod-twemoji"
```

The graphics will be mounted in `static/twemoji/png` and `static/twemoji/svg`, and the scripts in `static/twemoji/js`.

You'll want to call the script somewhere in your template or theme's `<head>`, for example:

```go
<script src="{{ "twemoji/js/twemoji.min.js" | absURL }}"></script>
```

...and then in order to actually swap out the emojis, you need to call the script's `.parse` method. This is where you can choose between PNGs or SVGs and tell the script where we've placed the graphics. A good starting point is:

```html
<script>
  twemoji.parse(document.body, {{ dict "base" ("/" | absURL) "folder" "twemoji/svg" "ext" ".svg" | jsonify | safeJS }})
</script>
```

After building the site this script will turn into something like:

```html
<script>
  twemoji.parse(document.body, {"base": "https://hugo-mod-twemoji.netlify.com/", "ext": ".svg", "folder": "twemoji/svg"})
</script>
```

## License

Twemoji graphics are licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC-BY-4.0) by Twitter, Inc. and other contributors. Code is licensed under the [MIT License](http://opensource.org/licenses/MIT).

Refer to the main [Twemoji repository](https://github.com/twitter/twemoji) or [website](https://twemoji.twitter.com/) for more information.
