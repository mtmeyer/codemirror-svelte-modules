<script lang="ts">
	import { onMount, createEventDispatcher } from 'svelte';
	import { EditorState } from '@codemirror/state';
	import type { Extension } from '@codemirror/state';
	import { EditorView } from '@codemirror/view';
	import { oneDark } from '@codemirror/theme-one-dark';
	import { basicSetup as basicSetupExtension } from '@codemirror/basic-setup';

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
	export let initialValue = 'Hello world';

	let classes = '';
	export { classes as class };

	let editorElement: HTMLDivElement;

	const dispatch = createEventDispatcher();

	let view;
	let state;

	const editableExtension = EditorView.editable.of(editable);
	const readOnlyExtension = EditorState.readOnly.of(readOnly);

	let updateListenerExtension = EditorView.updateListener.of((update) => {
		if (update.docChanged) {
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
