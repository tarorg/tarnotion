<script>
  import Editor from './components/Editor.svelte';
  import './global.css';

  let initialEditorData = {
    blocks: [
      {
        type: 'header',
        data: {
          text: 'Example Initial Content',
          level: 2,
        },
      },
      {
        type: 'paragraph',
        data: {
          text: 'This is some initial text for the editor.',
        },
      },
    ],
  };
</script>

<main>
  <Editor placeholder="Start writing here..." initialData={initialEditorData} />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>