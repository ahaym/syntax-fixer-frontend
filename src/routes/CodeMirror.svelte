<style unscoped>
:global(.cm-editor) {
  /* Set height, width, borders, and global font properties here */
  height: 500px;
  font-size: 16px;
}

:global(.cm-editor.cm-content.cm-line) {
  /* Set height, width, borders, and global font properties here */
  font-size: 16px;
}
</style>

<script lang="ts">
import { EditorView, basicSetup } from 'codemirror';
import { javascript } from '@codemirror/lang-javascript';
import { createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();

let editorPane;
export let textContent = '';

$: if (editorPane) {
  createEditor();
}

function createEditor() {
  let editor = new EditorView({
    doc: textContent,
    extensions: [basicSetup, javascript(), EditorView.updateListener.of(updateListener)],
    parent: editorPane
  });
}

function updateListener(v) {
  dispatch('change', {});
  textContent = v.state.doc.text.join('\n');
}
</script>

<div bind:this="{editorPane}" class=""></div>
