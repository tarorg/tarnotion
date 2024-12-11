<script>
  import EditorJS from '@editorjs/editorjs';
  import Header from '@editorjs/header';
  import List from '@editorjs/list';
  import Paragraph from '@editorjs/paragraph';
  import Checklist from '@editorjs/checklist';
  import Embed from '@editorjs/embed';
  import Table from '@editorjs/table';
  import ImageTool from '@editorjs/image';
  import Warning from '@editorjs/warning';
  import CodeTool from '@editorjs/code';
  import LinkTool from '@editorjs/link';
  import RawTool from '@editorjs/raw';
  import Quote from '@editorjs/quote';
  import Marker from '@editorjs/marker';
  import Delimiter from '@editorjs/delimiter';
  import InlineCode from '@editorjs/inline-code';
  import SimpleImage from '@editorjs/simple-image';

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
        paragraph: {
          class: Paragraph,
          inlineToolbar: true,
        },
        header: Header,
        list: List,
        checklist: Checklist,
        embed: Embed,
        table: Table,
        image: {
          class: ImageTool,
          config: {
            endpoints: {
              byFile: '/uploadFile', // Your backend file uploader endpoint
              byUrl: '/fetchUrl', // Your endpoint that provides uploading by Url
            }
          }
        },
        warning: Warning,
        code: CodeTool,
        linkTool: {
          class: LinkTool,
          config: {
            endpoint: '/fetchUrl', // Your backend endpoint for url data fetching
          }
        },
        raw: RawTool,
        quote: Quote,
        marker: Marker,
        delimiter: Delimiter,
        inlineCode: InlineCode,
        simpleImage: SimpleImage
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
<style>
  :global(.codex-editor *) {
    text-align: left;
  }
</style>