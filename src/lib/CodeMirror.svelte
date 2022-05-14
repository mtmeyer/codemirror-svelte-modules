<script lang="ts">
	import { onMount, createEventDispatcher } from 'svelte';
	import { EditorState } from '@codemirror/state';
	import type { Extension, EditorState as EditorStateType } from '@codemirror/state';
	import { EditorView, placeholder as placeholderExtension } from '@codemirror/view';
	import type { EditorView as EditorViewType } from '@codemirror/view';
	import { oneDark } from '@codemirror/theme-one-dark';
	import { basicSetup as basicSetupExtension } from '@codemirror/basic-setup';

	let state: EditorStateType;
	let view: EditorViewType;

	export let extensions: Extension[] = [];
	export let height = '';
	export let minHeight = '';
	export let maxHeight = '';
	export let width = '';
	export let minWidth = '';
	export let maxWidth = '';
	export let editable = true;
	export let readOnly = false;
	export let autoFocus = false;
	export let theme = oneDark;
	export let basicSetup = false;
	export let initialValue = '';
	export let currentValue = initialValue;
	export let placeholder = '';

	let classes = '';
	export { classes as class };

	let editorElement: HTMLDivElement;

	const dispatch = createEventDispatcher();

	const editableExtension = EditorView.editable.of(editable);
	const readOnlyExtension = EditorState.readOnly.of(readOnly);

	let updateListenerExtension = EditorView.updateListener.of((update) => {
		if (update.docChanged) {
			currentValue = update.state.doc.toString();
			dispatch('change', update);
		}
	});

	const initialExtensions = () => {
		let defaultExtensions = [
			defaultThemeOption,
			updateListenerExtension,
			editableExtension,
			readOnlyExtension,
			theme
		];
		if (basicSetup) defaultExtensions.push(basicSetupExtension);
		if (placeholder) defaultExtensions.push(placeholderExtension(placeholder));

		return defaultExtensions;
	};

	const defaultThemeOption = EditorView.theme({
		'&': {
			height,
			minHeight,
			maxHeight,
			width,
			minWidth,
			maxWidth
		}
	});

	onMount(async () => {
		state = EditorState.create({
			doc: initialValue,
			extensions: [...initialExtensions(), ...extensions]
		});

		view = new EditorView({
			state,
			parent: editorElement
		});

		if (autoFocus) view.focus();
	});
</script>

<div bind:this={editorElement} class={classes} />
