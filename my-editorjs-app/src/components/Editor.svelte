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
  import ColorPlugin from 'editorjs-text-color-plugin';
  import NestedList from '@editorjs/nested-list';
  import Tooltip from 'editorjs-tooltip';
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
              byFile: '/uploadFile',
              byUrl: '/fetchUrl',
            }
          }
        },
        warning: Warning,
        code: CodeTool,
        linkTool: {
          class: LinkTool,
          config: {
            endpoint: '/fetchUrl',
          }
        },
        raw: RawTool,
        quote: Quote,
        marker: Marker,
        delimiter: Delimiter,
        inlineCode: InlineCode,
        simpleImage: SimpleImage,
        Color: {
          class: ColorPlugin,
          config: {
            colorCollections: ['#EC7878','#9C27B0','#673AB7','#3F51B5','#0070FF','#03A9F4','#00BCD4','#4CAF50','#8BC34A','#CDDC39', '#FFEB3B', '#FFC107', '#FF9800', '#FF5722', '#795548', '#9E9E9E', '#607D8B'],
            defaultColor: '#FF1300',
            type: 'text',
            customPicker: true
          }     
        },
        Marker: {
          class: ColorPlugin,
          config: {
            defaultColor: '#FFBF00',
            type: 'marker',
            customPicker: true
          }
        },
      }
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