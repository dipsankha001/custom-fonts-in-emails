# custom-fonts-in-emails

[![build status](https://github.com/ladjs/custom-fonts-in-emails/actions/workflows/ci.yml/badge.svg)](https://github.com/ladjs/custom-fonts-in-emails/actions/workflows/ci.yml)
[![code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/sindresorhus/xo)
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
[![made with lass](https://img.shields.io/badge/made_with-lass-95CC28.svg)](https://lass.js.org)
[![license](https://img.shields.io/github/license/ladjs/custom-fonts-in-emails.svg)](LICENSE)

**An extremely easy way to use custom fonts in emails without having to use art software.**

* :art: Outputs optimized SVG, PNG, and Base64 inlined images with optional support for [**@2x**](https://github.com/2x) and [**@3x**](https://github.com/3x) Retina versions (uses the incredibly fast and performant [sharp][sharp]).
* :bulb: Automatic smart-detection of font names spelled incorrectly (or with the wrong extension) with 50% accuracy (uses [fast-levenshtein][fast-levenshtein] and checks for at least 50% distance match).
* :crystal\_ball: Detects user, local, network, system fonts, and `node_modules` folder fonts using [os-fonts][os-fonts] and [pkg-up][pkg-up] (e.g. you don't need to write `Arial.ttf`, you can just write `Arial`).
* :tada: Supports all WOFF, OTF, and TTF fonts (both with TrueType `glyf` and PostScript `cff` outlines).
* :sparkles: Use with recommended packages [nodemailer][nodemailer] and [nodemailer-base64-to-s3][nodemailer-base64-to-s3], or simply use [Lad][lad-url] (has this built-in).
* :pear: Pairs great with [font-awesome-assets][font-awesome-assets] and [juice][juice] (see [Lad's][lad-url] usage as an example).
* :white\_check\_mark: Supports offline and missing image support by automatically adding `alt`, `title`, and `style` attributes of `color` and `font-size` based upon the options passed.


## Index

* [What does this do?](#what-does-this-do)
  * [Old Approach](#old-approach)
  * [New Approach](#new-approach)
* [Examples](#examples)
* [Install](#install)
* [Usage](#usage)
* [Options](#options)
* [API](#api)
* [Wishlist](#wishlist)
* [Credits](#credits)
* [License](#license)

> **Don't want to configure this yourself?**  Try [Lad][lad-url]!


## What does this do

* Imagine you find a really cool font on GitHub such as [GoudyBookletter1911][goudybookletter1911], or another font at sites like [DaFont][dafont] or [Font Squirrel][font-squirrel].
* You want to use this font in your emails and write the text "Make something people want" with it to use as a footer graphic.  **:tada: Congratulations, because this package lets you do that with basically one line of code!**
* Let's compare the old and new way (thanks to this package) to put custom fonts in emails.

### Old Approach

Here's the old, slow, and convoluted way you'd do this:

1. Typically you'd have to open Photoshop, GIMP, or Sketch (wait for the updates to finish), and then create an image with this text, select the font, color, and then save it as an image.
2. Then upload it somewhere or have to wait until it deploys to production so you have a valid non-local URL (which is prone to caching in Gmail – in other words... if you ever need to make a slight adjustment to it then you have to completely rename the file).
3. Reference the image in your HTML and try to rememember it's dimensions, or have to open up the art software again to get dimensions. What about Retina? What if you need to change the size or color of the font? What if you need to convert points to pixels? Just forget it...  It's too complicated and time consuming, and now your emails will look boring like they always did! :frowning: :rage:

### New Approach

:boom: You don't need to do that anymore! :smile: Here's how easy it is:

```js
import customFonts from 'custom-fonts-in-emails';
import path from 'path';

const options = {
  text: 'Make something people want',
  fontNameOrPath: 'GoudyBookletter1911',
  fontColor: 'white',
  backgroundColor: '#ff6600',
  fontSize: 40
};

customFonts.png2x(options)
  .then(console.log)
  .catch(console.error);
```

```html
<img width="461" height="51" src="data:image/png;base64,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" title="Make something people want" alt="Make something people want" style="color: white;font-size: 25.5px;line-height: 51px;text-align: center;background-color: #ff6600;">
```

<img width="461" height="51" src="https://raw.githubusercontent.com/ladjs/custom-fonts-in-emails/master/art/png@2x.png" title="Make something people want" alt="Make something people want" style="color: white;font-size: 25.5px;line-height: 51px;text-align: center;background-color: #ff6600;">

You can now use any font in your emails – without having to use art software like Photoshop or Sketch!

It supports system-wide and `node_modules` fonts out of the box, but you can pass a file path if you wish to use a custom non-standard font.  You can also customize its kerning, anchor, color/fill, stroke, font size (even in points if needed), add custom attributes to the HTML tag, and more!  See [Usage](#usage), [Options](#options), and the [API](#api) reference below for more info.

It even uses the [fast-levenshtein][fast-levenshtein] algorithm to detect the closest match to the spelling of a font (e.g. in case you mispellled `Arial` as `Arail`).


## Examples

Using the options defined in [New Approach](#new-approach) above, the following code provides examples of this package's [API](#api) methods.

| API Method and Preview                                                                                                                                                                                                                                                                                                           | Image Type                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `customFonts.svg(options)` <img width="461" height="51" src="https://raw.githubusercontent.com/ladjs/custom-fonts-in-emails/master/art/image.svg" style="background-color: #ff6600;color: white;font-size: 20px;line-height: 40px;text-align: center;" title="Make something people want" alt="Make something people want">      | SVG tag `<svg>`                                                                            |
| `customFonts.img(options)` <img width="461" height="51" src="https://raw.githubusercontent.com/ladjs/custom-fonts-in-emails/master/art/img.svg" style="background-color: #ff6600;color: white;font-size: 20px;line-height: 40px;text-align: center;" title="Make something people want" alt="Make something people want">        | IMG tag `<img>` with Base64-encoded Inline SVG                                             |
| `customFonts.png(options, scale)` <img width="461" height="51" src="https://raw.githubusercontent.com/ladjs/custom-fonts-in-emails/master/art/png.png" style="background-color: #ff6600;color: white;font-size: 20px;line-height: 40px;text-align: center;" title="Make something people want" alt="Make something people want"> | IMG tag `<img>` with Base64-encoded Inline PNG                                             |
| `customFonts.png@2x(options)` <img width="461" height="51" src="https://raw.githubusercontent.com/ladjs/custom-fonts-in-emails/master/art/png@2x.png" style="background-color: #ff6600;color: white;font-size: 20px;line-height: 40px;text-align: center;" title="Make something people want" alt="Make something people want">  | IMG tag `<img>` with Base64-encoded Inline PNG [**@2x**](https://github.com/2x) Resolution |
| `customFonts.png@3x(options)` <img width="461" height="51" src="https://raw.githubusercontent.com/ladjs/custom-fonts-in-emails/master/art/png@3x.png" style="background-color: #ff6600;color: white;font-size: 20px;line-height: 40px;text-align: center;" title="Make something people want" alt="Make something people want">  | IMG tag `<img>` with Base64-encoded Inline PNG [**@3x**](https://github.com/3x) Resolution |

Lastly, here's what a broken image looks like that was attempted to be rendered with an API method.  It makes use of the option `supportsFallback` defined below in [Options](#options).  This is a really useful fallback for offline emails, invalid cached images, and more!

<img width="461" height="51" src="https://raw.githubusercontent.com/ladjs/custom-fonts-in-emails/master/art/fallback@2x.png" title="Make something people want" alt="Make something people want">


## Install

```bash
npm install -s custom-fonts-in-emails
```


## Usage

```js
import customFonts from 'custom-fonts-in-emails';
```

or

```js
import {
  setDefaults,
  setOptions,
  svg,
  img,
  png,
  png2x,
  png3x,
  getClosestFontName,
  getFontPathsByName,
  getFontPathByName,
  getAvailableFontPaths,
  getAvailableFontNames,
  // optional: this is a cache of all the custom fonts loaded
  customFontsCache
} from 'custom-fonts-in-emails';
```

If you plan to use this for outbound emails, then you'll want to make use of [nodemailer][nodemailer] and [nodemailer-base64-to-s3][nodemailer-base64-to-s3].  Or you can simply use [Lad][lad-url], which has this built-in already!


## Options

The `options` argument in all [API](#api) methods is an Object that accepts the following properties:

| Property           |       Type       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------ | :--------------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `text`             |      String      | Text to write using the font family specified in `fontNameOrPath` (defaults to an empty String of `''`)                                                                                                                                                                                                                                                                                                                                               |
| `fontNameOrPath`   |      String      | Name or file path of the font (defaults to `Arial` – note that by default we load user, local, network, system, and `node_modules` fonts across any operating system using [os-fonts][os-fonts], so you can use any font installed!)                                                                                                                                                                                                                  |
| `fontSize`         | Number or String | Size of font **in pixels**, which is rounded to nearest whole Integer (this automatically sets `options.textToSvg.fontSize` – defaults to `24px`, but you don't need to specify the affix `px` as it is automatically stripped and converted to the nearest whole Integer using `Math.round(parseInt(val, 10))` – this value must be greater than 0)                                                                                                  |
| `fontColor`        |      String      | Valid hex color or rgba value to render the text fill color with with (defaults to `#000`)                                                                                                                                                                                                                                                                                                                                                            |
| `backgroundColor`  |      String      | Valid hex color or rgba value to render the background color with (defaults to `transparent`)                                                                                                                                                                                                                                                                                                                                                         |
| `supportsFallback` |      Boolean     | Ensure that the output image has fallback attributes `title` and `alt` (both set to the value of `options.text`), and `style` which is set to or concatenated with `color` set to `options.fontColor`, `font-size` set to `options.fontSize / 2` (for vertical centering we divide by 2), `line-height` set to `options.fontSize` affixed with `px`, and finally `text-align: center` &ndash not applicable to `customFonts.svg` (defaults to `true`) |
| `resizeToFontSize` |      Boolean     | Ensure that the output image height is resized to `fontSize`, and its width is proportionally scaled – not applicable to `customFonts.svg` nor `customFonts.img` (defaults to `false`)                                                                                                                                                                                                                                                                |
| `trim`             |      Boolean     | Ensure that the output image is trimmed using [sharp][sharp]'s [trim][trim] API method – it trims "boring" pixels from the edges – not applicable to `customFonts.svg` nor `customFonts.img` (defaults to `false`)                                                                                                                                                                                                                                    |
| `trimTolerance`    |      Number      | Must be from 1-99 inclusive, sets the trim tolerance value using [trim][trim] (defaults to `10`)                                                                                                                                                                                                                                                                                                                                                      |
| `attrs`            |      Object      | Attribute key-value pairs that will be applied to the returned tag (defaults to `{}`, e.g. if you want to make the image output a fixed height scaled proportionally, then you can do `{ style: 'height: 40px; width: auto;' }`, **this is useful if you want to add custom CSS classes, style attributes, or other attributes in general to the returned tags**)                                                                                     |
| `textToSvg`        |      Object      | Options defined in [textToSvg](#texttosvg) below which get passed to [text-to-svg][text-to-svg] (and subsequently [opentype.js][opentype.js]):                                                                                                                                                                                                                                                                                                        |

### textToSvg

| Property     |  Type  | Description                                                                                                                                                                                                                                                                                      |
| ------------ | :----: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `x`          | Number | Horizontal position of the beginning of the text (defaults to `0`)                                                                                                                                                                                                                               |
| `y`          | Number | Vertical position of the baseline of the text (defaults to `0`)                                                                                                                                                                                                                                  |
| `fontSize`   | Number | Size of the text in points (defaults to `options.fontSize`)                                                                                                                                                                                                                                      |
| `anchor`     | String | Anchor of object in coordinate (defaults to `left top` – the String consists of `horizontal vertical`, where `horizontal` can be one of `left`, `center`, or `right`, and `vertical` can be one of `baseline`, `top`, `middle`, `bottom`)                                                        |
| `attributes` | Object | Attribute key-value pairs that will be applied to the returned `<path>` element inside the `<svg>` tag (defaults to `{ fill: '#000', stroke: 'none' }` – note that if you specify `fontColor` then it will set `fill` equal to `fontColor`, but it can be overridden this attribute explicitly!) |


## API

> Please note that as of `v1.0.0` this API is synchronous and will block.  For an asynchronous version please use `v0.0.4` of this package.

### `customFonts.setDefaults(options)`

A function that accepts [options](#options) to set defaults for future use and returns a Promise that resolveswith the new package defaults.

### `customFonts.setOptions(options)`

A function that accepts [options](#options) and returns a Promise that resolves with refined `options`.

### `customFonts.svg(options)`

A function that accepts [options](#options) and returns a Promise that resolves with a String of the `<svg>` HTML tag for the custom font.

This function takes the argument `options` and passes it to `customFonts.setOptions`.

### `customFonts.img(options)`

Same as `customFonts.svg`, except it returns the String as Base64 inlined data.

### `customFonts.png(options, scale)`

Same as `customFonts.img`, except it returns Base64 inlined data for a PNG instead of an SVG.

It also optionally accepts a Number `scale` (defaults to `1`), which will scale the image for retina support.

For example, if the font rendered is 20px, then it will multiply 20px by `scale` (e.g. if `scale` is `2`, then the image returned will be `20px` but it will be scaled to `40px`).

### `customFonts.png2x(options)`

Same as `customFonts.png`, except it returns an image with twice as many pixels (it multiplies `fontSize * 2` and returns an image scaled to 1x for 2x retina support, it passes `2` as the `scale`).

### `customFonts.png3x(options)`

Same as `customFonts.png`, except it returns an image with three as many pixels (it multiplies `fontSize * 3` and returns an image scaled to 1x for 3x retina support, it passes `3` as the `scale`).

### `customFonts.getAvailableFontPaths()`

A function that returns an Array of file paths for all of the user, local, network, system, and `node_modules` fonts available on the current operating system.

### `customFonts.getAvailableFontNames()`

The same as `customFonts.getAvailableFontPaths`, except it returns font names instead of font paths.

### `customFonts.customFontsCache`

This is an object of all of the custom fonts cached.


## Wishlist

* \[ ] [svg/svgo#620](https://github.com/svg/svgo/issues/620)
* \[ ] [svg/svgo#619](https://github.com/svg/svgo/issues/619)
* \[ ] [nodebox/opentype.js#238](https://github.com/nodebox/opentype.js/issues/238)
* \[ ] [jergason/recursive-readdir#35](https://github.com/jergason/recursive-readdir/pull/35)
* \[ ] [shrhdk/text-to-svg#20](https://github.com/shrhdk/text-to-svg/issues/20)
* \[ ] [shrhdk/text-to-svg#19](https://github.com/shrhdk/text-to-svg/issues/19)
* \[ ] [shrhdk/text-to-svg#18](https://github.com/shrhdk/text-to-svg/issues/18)


## Credits

Thanks to the public domain font [GoudyBookletter1911][goudybookletter1911] for test purpose and our friends in [the Slack channel][slack-url] for support.


## License

[MIT](LICENSE) © Nick Baugh


##

[lad-url]: https://lad.js.org

[slack-url]: https://join.slack.com/t/ladjs/shared_invite/zt-fqei6z11-Bq2trhwHQxVc5x~ifiZG0g

[nodemailer]: https://github.com/nodemailer/nodemailer

[nodemailer-base64-to-s3]: https://github.com/ladjs/nodemailer-base64-to-s3

[dafont]: http://www.dafont.com/

[font-squirrel]: https://www.fontsquirrel.com/

[font-awesome-assets]: https://github.com/ladjs/font-awesome-assets

[text-to-svg]: https://github.com/shrhdk/text-to-svg

[opentype.js]: https://github.com/nodebox/opentype.js

[os-fonts]: https://github.com/vutran/os-fonts

[fast-levenshtein]: https://github.com/hiddentao/fast-levenshtein

[goudybookletter1911]: https://github.com/theleagueof/goudy-bookletter-1911

[juice]: https://github.com/Automattic/juice

[sharp]: https://github.com/lovell/sharp

[trim]: http://sharp.dimens.io/en/stable/api/#trimtolerance

[pkg-up]: https://github.com/sindresorhus/pkg-up
