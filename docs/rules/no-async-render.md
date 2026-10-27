# lit-robust/no-async-render

📝 Disallow an `async` `render()` on Lit components.

💼 This rule is enabled in the 🧩 `core` [config](https://github.com/cunarist/eslint-plugin-lit-robust#configs-and-rules).

<!-- end auto-generated rule header -->

## Rule Details

An `async` method returns a Promise, and Lit commits whatever `render()` returns
directly into the DOM. A Promise is not a template, node, or iterable, so Lit
falls back to its string form and the component shows the literal text
`[object Promise]` instead of your template, every render. The awaited result
is never rendered. Fetch the data elsewhere, store it in a reactive property, and
render that synchronously.

- Only `render` is checked. Async lifecycle hooks are covered by
  [`no-async-lifecycle`](no-async-lifecycle.md).
- A `static render()` is not a lifecycle method and is ignored.
- Only classes that extend `LitElement` or `ReactiveElement` are checked.

Examples of **incorrect** code for this rule:

```ts
class El extends LitElement {
  async render() {
    const items = await this.load();
    return html`<p>${items}</p>`;
  }
}
```

Examples of **correct** code for this rule:

```ts
class El extends LitElement {
  @state()
  accessor items = [];

  render() {
    return html`<p>${this.items}</p>`;
  }
}
```

## When Not To Use It

There is no case where Lit renders a Promise returned by `render()`, so keep it
on.
