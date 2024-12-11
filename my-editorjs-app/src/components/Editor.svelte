<script>
  import EditorJS from '@editorjs/editorjs';
  import Header from '@editorjs/header';
  import List from '@editorjs/list';
  import Paragraph from '@editorjs/paragraph';
  import { onMount, onDestroy } from 'svelte';

  export let placeholder = 'Tell your story...';
  export let initialData = null;

  let editorInstance = null;
  let editorContainer;

  onMount(() => {
    editorInstance = new EditorJS({
      holder: editorContainer,
      placeholder: placeholder,
      tools: {
        header: Header,
        list: List,
        paragraph: {
          class: Paragraph,
          inlineToolbar: true,
        },
      },
      data: initialData,
    });
  });

  onDestroy(() => {
    if (editorInstance && editorInstance.destroy) {
      editorInstance.destroy();
    }
  });

  async function handleSave() {
    const outputData = await editorInstance.save();
    console.log('Article data: ', outputData);
    return outputData;
  }
</script>

<div bind:this={editorContainer} />

<button on:click={handleSave}>Save</button>

<style>
  :global(.codex-editor *) {
    text-align: left;
  }
</style>