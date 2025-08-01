---
title: "@media"
slug: Web/CSS/@media
page-type: css-at-rule
browser-compat: css.at-rules.media
sidebar: cssref
---

The **`@media`** [CSS](/en-US/docs/Web/CSS) [at-rule](/en-US/docs/Web/CSS/CSS_syntax/At-rule) can be used to apply part of a style sheet based on the result of one or more [media queries](/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries). With it, you specify a media query and a block of CSS to apply to the document if and only if the media query matches the device on which the content is being used.

> [!NOTE]
> In JavaScript, the rules created using `@media` can be accessed with the {{domxref("CSSMediaRule")}} CSS object model interface.

{{InteractiveExample("CSS Demo: @media", "tabbed-standard")}}

```css interactive-example
abbr {
  color: #860304;
  font-weight: bold;
  transition: color 0.5s ease;
}

@media (hover: hover) {
  abbr:hover {
    color: #001ca8;
    transition-duration: 0.5s;
  }
}

@media not all and (hover: hover) {
  abbr::after {
    content: " (" attr(title) ")";
  }
}
```

```html interactive-example
<p>
  <abbr title="National Aeronautics and Space Administration">NASA</abbr> is a
  U.S. government agency that is responsible for science and technology related
  to air and space.
</p>
```

## Syntax

```css
/* At the top level of your code */
@media screen and (width >= 900px) {
  article {
    padding: 1rem 3rem;
  }
}

/* Nested within another conditional at-rule */
@supports (display: flex) {
  @media screen and (width >= 900px) {
    article {
      display: flex;
    }
  }
}
```

The `@media` at-rule may be placed at the top level of your code or nested inside any other conditional group at-rule.

For a discussion of media query syntax, please see [Using media queries](/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries#syntax).

## Description

A media query's `<media-query-list>` includes [`<media-type>`s](#media_types), [`<media-feature>s`](#media_features), and [logical operators](#logical_operators).

### Media types

A _`<media-type>`_ describes the general category of a device.
Except when using the `only` logical operator, the media type is optional and the `all` type is implied.

- `all`
  - : Suitable for all devices.
- `print`
  - : Intended for paged material and documents viewed on a screen in print preview mode. (Please see [paged media](/en-US/docs/Web/CSS/CSS_paged_media) for information about formatting issues that are specific to these formats.)
- `screen`
  - : Intended primarily for screens.

> [!NOTE]
> CSS2.1 and [Media Queries 3](https://drafts.csswg.org/mediaqueries-3/#background) defined several additional media types (`tty`, `tv`, `projection`, `handheld`, `braille`, `embossed`, and `aural`, but they were deprecated in [Media Queries 4](https://drafts.csswg.org/mediaqueries/#media-types) and shouldn't be used.

### Media features

A _`<media feature>`_ describes specific characteristics of the {{glossary("user agent")}}, output device, or environment.
Media feature expressions test for their presence, value, or range of values, and are entirely optional. Each media feature expression must be surrounded by parentheses.

- {{cssxref("@media/any-hover", "any-hover")}}
  - : Does any available input mechanism allow the user to hover over elements?
- {{cssxref("@media/any-pointer", "any-pointer")}}
  - : Is any available input mechanism a pointing device, and if so, how accurate is it?
- {{cssxref("@media/aspect-ratio", "aspect-ratio")}}
  - : Width-to-height {{glossary("aspect ratio")}} of the viewport.
- {{cssxref("@media/color", "color")}}
  - : Number of bits per color component of the output device, or zero if the device isn't color.
- {{cssxref("@media/color-gamut", "color-gamut")}}
  - : Approximate range of colors that are supported by the user agent and output device.
- {{cssxref("@media/color-index", "color-index")}}
  - : Number of entries in the output device's color lookup table, or zero if the device does not use such a table.
- {{cssxref("@media/device-aspect-ratio", "device-aspect-ratio")}}
  - : Width-to-height aspect ratio of the output device. Deprecated in Media Queries Level 4.
- {{cssxref("@media/device-height", "device-height")}}
  - : Height of the rendering surface of the output device. Deprecated in Media Queries Level 4.
- {{cssxref("@media/device-posture", "device-posture")}}
  - : Detects the device's current posture, that is, whether the viewport is in a flat or folded state. Defined in the [Device Posture API](/en-US/docs/Web/API/Device_Posture_API).
- {{cssxref("@media/device-width", "device-width")}}
  - : Width of the rendering surface of the output device. Deprecated in Media Queries Level 4.
- {{cssxref("@media/display-mode", "display-mode")}}
  - : The mode in which an application is being displayed: for example, [fullscreen](/en-US/docs/Web/CSS/@media/display-mode#fullscreen) or [picture-in-picture](/en-US/docs/Web/CSS/@media/display-mode#picture-in-picture) mode.
    Added in Media Queries Level 5.
- {{cssxref("@media/dynamic-range", "dynamic-range")}}
  - : Combination of brightness, contrast ratio, and color depth that are supported by the user agent and the output device. Added in Media Queries Level 5.
- {{cssxref("@media/forced-colors", "forced-colors")}}
  - : Detect whether user agent restricts color palette.
    Added in Media Queries Level 5.
- {{cssxref("@media/grid", "grid")}}
  - : Does the device use a grid or bitmap screen?
- {{cssxref("@media/height", "height")}}
  - : Height of the viewport.
- {{cssxref("@media/hover", "hover")}}
  - : Does the primary input mechanism allow the user to hover over elements?
- {{cssxref("@media/inverted-colors", "inverted-colors")}}
  - : Is the user agent or underlying OS inverting colors?
    Added in Media Queries Level 5.
- {{cssxref("@media/monochrome", "monochrome")}}
  - : Bits per pixel in the output device's monochrome frame buffer, or zero if the device isn't monochrome.
- {{cssxref("@media/orientation", "orientation")}}
  - : Orientation of the viewport.
- {{cssxref("@media/overflow-block", "overflow-block")}}
  - : How does the output device handle content that overflows the viewport along the block axis?
- {{cssxref("@media/overflow-inline", "overflow-inline")}}
  - : Can content that overflows the viewport along the inline axis be scrolled?
- {{cssxref("@media/pointer", "pointer")}}
  - : Is the primary input mechanism a pointing device, and if so, how accurate is it?
- {{cssxref("@media/prefers-color-scheme", "prefers-color-scheme")}}
  - : Detect if the user prefers a light or dark color scheme.
    Added in Media Queries Level 5.
- {{cssxref("@media/prefers-contrast", "prefers-contrast")}}
  - : Detects if the user has requested the system increase or decrease the amount of contrast between adjacent colors.
    Added in Media Queries Level 5.
- {{cssxref("@media/prefers-reduced-data", "prefers-reduced-data")}}
  - : Detects if the user has requested the web content that consumes less internet traffic.
- {{cssxref("@media/prefers-reduced-motion", "prefers-reduced-motion")}}
  - : The user prefers less motion on the page.
    Added in Media Queries Level 5.
- {{cssxref("@media/prefers-reduced-transparency", "prefers-reduced-transparency")}}
  - : Detects if a user has enabled a setting on their device to reduce the transparent or translucent layer effects used on the device.
- {{cssxref("@media/resolution", "resolution")}}
  - : Pixel density of the output device.
- {{cssxref("@media/scan", "scan")}}
  - : Whether display output is progressive or interlaced.
- {{cssxref("@media/scripting", "scripting")}}
  - : Detects whether scripting (i.e., JavaScript) is available.
    Added in Media Queries Level 5.
- {{cssxref("@media/shape", "shape")}}
  - : Detects the shape of the device to distinguish rectangular and round displays.
- {{cssxref("@media/update", "update")}}
  - : How frequently the output device can modify the appearance of content.
- {{cssxref("@media/video-dynamic-range", "video-dynamic-range")}}
  - : Combination of brightness, contrast ratio, and color depth that are supported by the video plane of user agent and the output device. Added in Media Queries Level 5.
- {{cssxref("@media/width", "width")}}
  - : Width of the viewport including width of scrollbar.
- {{cssxref("@media/-moz-device-pixel-ratio", "-moz-device-pixel-ratio")}}
  - : The number of device pixels per CSS pixel. Use the [`resolution`](/en-US/docs/Web/CSS/@media/resolution) feature with the `dppx` unit instead.
- {{cssxref("@media/-webkit-animation", "-webkit-animation")}}
  - : The browser supports `-webkit` prefixed CSS {{cssxref("animation")}}. Use the [`@supports (animation)`](/en-US/docs/Web/CSS/@supports) feature query instead.
- {{cssxref("@media/-webkit-device-pixel-ratio", "-webkit-device-pixel-ratio")}}
  - : The number of device pixels per CSS pixel. Use the [`resolution`](/en-US/docs/Web/CSS/@media/resolution) feature with the `dppx` unit instead.
- {{cssxref("@media/-webkit-transform-2d", "-webkit-transform-2d")}}
  - : The browser supports `-webkit` prefixed 2D CSS {{cssxref("transform")}}. Use the [`@supports (transform)`](/en-US/docs/Web/CSS/@supports) feature query instead.
- {{cssxref("@media/-webkit-transform-3d", "-webkit-transform-3d")}}
  - : The browser supports `-webkit` prefixed 3D CSS {{cssxref("transform")}}. Use the [`@supports (transform)`](/en-US/docs/Web/CSS/@supports) feature query instead.
- {{cssxref("@media/-webkit-transition", "-webkit-transition")}}
  - : The browser supports `-webkit` prefixed CSS {{cssxref("transition")}}. Use the [`@supports (transition)`](/en-US/docs/Web/CSS/@supports) feature query instead.

### Logical operators

The _logical operators_ `not`, `and`, `only`, and `or` can be used to compose a complex media query.
You can also combine multiple media queries into a single rule by separating them with commas.

- `and`
  - : Used for combining multiple media features together into a single media query, requiring each chained feature to return `true` for the query to be `true`.
    It is also used for joining media features with media types.
- `not`
  - : Used to negate a media query, returning `true` if the query would otherwise return `false`.
    If present in a comma-separated list of queries, it will only negate the specific query to which it is applied.

    > [!NOTE]
    > In Level 3, the `not` keyword can't be used to negate an individual media feature expression, only an entire media query.

- `only`
  - : Applies a style only if an entire query matches.
    It is useful for preventing older browsers from applying selected styles.
    When not using `only`, older browsers would interpret the query `screen and (width <= 500px)` as `screen`, ignoring the remainder of the query, and applying its styles on all screens.
    If you use the `only` operator, you _must also_ specify a media type.
- `,` (comma)
  - : Commas are used to combine multiple media queries into a single rule.
    Each query in a comma-separated list is treated separately from the others
    Thus, if any of the queries in a list is `true`, the entire media statement returns `true`.
    In other words, lists behave like a logical `or` operator.
- `or`
  - : Equivalent to the `,` operator. Added in Media Queries Level 4.

### User agent client hints

Some media queries have corresponding [user agent client hints](/en-US/docs/Web/HTTP/Guides/Client_hints).
These are HTTP headers that request content that is pre-optimized for the particular media requirement.
They include {{HTTPHeader("Sec-CH-Prefers-Color-Scheme")}} and {{HTTPHeader("Sec-CH-Prefers-Reduced-Motion")}}.

## Formal syntax

{{csssyntax}}

## Accessibility

To best accommodate people who adjust a site's text size, use [`em`](/en-US/docs/Web/CSS/CSS_Values_and_Units/Numeric_data_types)s when you need a {{cssxref("&lt;length&gt;")}} for your [media queries](/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries).

Both [`em`](/en-US/docs/Web/CSS/CSS_Values_and_Units/Numeric_data_types) and [`px`](/en-US/docs/Web/CSS/CSS_Values_and_Units/Numeric_data_types) are valid units, but [`em`](/en-US/docs/Web/CSS/CSS_Values_and_Units/Numeric_data_types) works better if the user changes the browser text size.

Also consider media queries or [HTTP user agent client hints](/en-US/docs/Web/HTTP/Guides/Client_hints#user_agent_client_hints) to improve the user's experience.
For example, the media query [`prefers-reduced-motion`](/en-US/docs/Web/CSS/@media/prefers-reduced-motion) or the equivalent HTTP header {{HTTPHeader("Sec-CH-Prefers-Reduced-Motion")}}) can be used to minimize the amount of animation or motion used based on user preferences.

## Security

Because media queries provide insights into the capabilities—and by extension, the features and design—of the device the user is working with, there is the potential that they could be abused to construct a ["fingerprint"](/en-US/docs/Glossary/Fingerprinting) which identifies the device, or at least categorizes it to some degree of detail that may be undesirable to users.

Because of this potential, a browser may opt to fudge the returned values in some manner in order to prevent them from being used to precisely identify a computer. A browser might also offer additional measures in this area; for example, if Firefox's "Resist Fingerprinting" setting is enabled, many media queries report default values rather than values representing the actual device state.

## Examples

### Testing for print and screen media types

```css
@media print {
  body {
    font-size: 10pt;
  }
}

@media screen {
  body {
    font-size: 13px;
  }
}

@media screen, print {
  body {
    line-height: 1.2;
  }
}
```

The range syntax allows for less verbose media queries when testing for any feature accepting a range, as shown in the below examples:

```css
@media (height > 600px) {
  body {
    line-height: 1.4;
  }
}

@media (400px <= width <= 700px) {
  body {
    line-height: 1.4;
  }
}
```

For more examples, please see [Using media queries](/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries).

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [CSS media queries](/en-US/docs/Web/CSS/CSS_media_queries) module
- [Using media queries](/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries)
- {{domxref("CSSMediaRule")}} interface
- [Extended Mozilla media features](/en-US/docs/Web/CSS/Mozilla_Extensions#media_features)
- [Extended WebKit media features](/en-US/docs/Web/CSS/WebKit_Extensions#media_features)
