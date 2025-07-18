---
title: text-align-last
slug: Web/CSS/text-align-last
page-type: css-property
browser-compat: css.properties.text-align-last
sidebar: cssref
---

The **`text-align-last`** [CSS](/en-US/docs/Web/CSS) property sets how the last line of a block or a line, right before a forced line break, is aligned.

{{InteractiveExample("CSS Demo: text-align-last")}}

```css interactive-example-choice
text-align-last: right;
```

```css interactive-example-choice
text-align-last: center;
```

```css interactive-example-choice
text-align-last: left;
```

```html interactive-example
<section id="default-example">
  <div>
    <p id="example-element">
      Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
      aliquip ex ea commodo consequat.
    </p>
  </div>
</section>
```

```css interactive-example
section {
  font-size: 1.5em;
}

#default-example > div {
  width: 250px;
}

#example-element {
  text-align: justify;
}
```

## Syntax

```css
/* Keyword values */
text-align-last: auto;
text-align-last: start;
text-align-last: end;
text-align-last: left;
text-align-last: right;
text-align-last: center;
text-align-last: justify;

/* Global values */
text-align-last: inherit;
text-align-last: initial;
text-align-last: revert;
text-align-last: revert-layer;
text-align-last: unset;
```

### Values

- `auto`
  - : The affected line is aligned per the value of {{cssxref("text-align")}}, unless {{cssxref("text-align")}} is `justify`, in which case the effect is the same as setting `text-align-last` to `start`.
- `start`
  - : The same as `left` if direction is left-to-right and `right` if direction is right-to-left.
- `end`
  - : The same as `right` if direction is left-to-right and `left` if direction is right-to-left.
- `left`
  - : The inline contents are aligned to the left edge of the line box.
- `right`
  - : The inline contents are aligned to the right edge of the line box.
- `center`
  - : The inline contents are centered within the line box.
- `justify`
  - : The text is justified. Text should line up their left and right edges to the left and right content edges of the paragraph.

## Formal definition

{{CSSInfo}}

## Formal syntax

{{csssyntax}}

## Examples

### Justifying the last line

```html hidden
<p>
  Integer elementum massa at nulla placerat varius. Suspendisse in libero risus,
  in interdum massa. Vestibulum ac leo vitae metus faucibus gravida ac in neque.
  Nullam est eros, suscipit sed dictum quis, accumsan a ligula.
</p>
```

#### CSS

```css
p {
  font-size: 1.4em;
  text-align: justify;
  text-align-last: center;
}
```

#### Results

{{EmbedLiveSample('Justifying_the_last_line','560')}}

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{cssxref("text-align")}}
