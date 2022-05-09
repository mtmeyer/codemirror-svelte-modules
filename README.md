# CodeMirror Svelte Modules

This is a simple wrapper around Codemirror using their new architecture in v6.

This library was heavily inspired by a similar React library [`@uiw/react-codemirror`](https://uiwjs.github.io/react-codemirror/)

## CodeMirror Documentation

In version 6 CodeMirror only exports modules as described here: [CodeMirror v6 Guide](https://codemirror.net/6/docs/guide/)

For all the v6 documentation go here: [CodeMirror v6 Documentation](https://codemirror.net/6/docs/)

## Example

```html
<script>
	import CodeMirror from 'codemirror-svelte-modules';
</script>

<CodeMirror basicSetup />
```

### Props

More details on props and their types coming soon

- extensions: `Extensions[]`
- height: `string`
- minHeight: `string`
- maxHeight: `string`
- width: `string`
- minWidth: `string`
- maxWidth: `string`
- editable: `boolean`
- readOnly: `boolean`
- autoFocus: `boolean`
- theme: `Extension`
- basicSetup: `boolean`
- initialValue: `string`

### Adding extensions

```html
<script>
	import CodeMirror from 'codemirror-svelte-modules';
	import { syntaxHighlighting, defaultHighlightStyle } from '@codemirror/language';
	import { javascript } from '@codemirror/lang-javascript';
</script>

<CodeMirror extensions={[javascript(), syntaxHighlighting(defaultHighlightStyle)]} />
```
