# Lake HTML

[![CI](https://github.com/lakejs/lake-html/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/lakejs/lake-html/actions)
[![npm](https://img.shields.io/npm/v/lake-html)](https://www.npmjs.com/package/lake-html)
[![downloads](https://img.shields.io/npm/dm/lake-html)](https://www.npmjs.com/package/lake-html)

---

A lightweight, zero-dependency utility to convert Lake Markup Language (LML) strings into standard, semantic HTML.

Designed for [Lake](https://lakejs.org/), this library handles the parsing of custom `<lake-box>` tags and transforms them into clean HTML elements like images, code blocks, embeds, and more.

## Features

* **Lightweight**: Zero external dependencies.

* **Isomorphic**: Works perfectly in Node.js and Browsers.

* **Safe**: Auto-encodes HTML entities to prevent XSS.

* **Extensible**: Easily override default renderers or add custom box types.

* **TypeScript**: Written in TypeScript with complete type definitions.

## Installation

```bash
npm install lake-html
```

## Basic Usage

Import the `toHTML` function and pass your [LML](https://lakejs.org/guide/content-format) string.

``` js
import { toHTML } from 'lake-html';

const content = `
  <p>Hello World</p>
  <lake-box name="image" value="eyJ1cmwiOi..."></lake-box>
  <p>End of content</p>
`;

const html = toHTML(content);

console.log(html);
// Output:
// <p>Hello World</p>
// <img src="..." border="0" />
// <p>End of content</p>
```

## Customization

You can customize how specific boxes are rendered or add support for new box types by passing a `renderers` object as the second argument.

### Overriding an existing renderer

For example, if you want to wrap all images in a `<figure>` tag:

```js
import { toHTML, getBoxRenderers } from 'lake-html';

const renderers = getBoxRenderers();

// Override the 'image' renderer
renderers.image = (boxValue, encode) => {
  return {
    tagName: 'figure',
    attributes: { class: 'custom-image' },
    innerHTML: `<img src="${encode(boxValue.url)}" alt="${encode(boxValue.caption)}" />`
  };
};

const html = toHTML(content, renderers);
```

### Adding a new box type

```js
import { toHTML, getBoxRenderers } from 'lake-html';

const renderers = getBoxRenderers();

// Add a custom 'card' box
renderers.card = (boxValue, encode) => {
  return `<div class="card">
    <h3>${encode(boxValue.title)}</h3>
    <p>${encode(boxValue.summary)}</p>
  </div>`;
};

const html = toHTML(content, renderers);
```

## API Reference

### toHTML(value: string, renderers?: BoxRenderers): string

The main conversion function.

* `value`: The input LML string.

* `renderers`: (Optional) An object to override default renderers.

### getBoxRenderers()

Returns the default map of box renderers. Useful when you want to extend the defaults rather than replace them entirely.

## Development

```bash
# Build the library
pnpm build

# Run tests
pnpm test
```
