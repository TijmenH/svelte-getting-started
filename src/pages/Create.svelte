<script>
  import { navigate } from 'svelte-routing';

  import BackButtonRow from "../components/BackButtonRow/BackButtonRow.svelte";
  import BookCover from "../components/BookCover/BookCover.svelte";
  import Button from "../components/Button/Button.svelte";
  import Header from "../components/Header/Header.svelte";
  import TextInput from '../components/TextInput/TextInput.svelte';
  import { httpPost } from "../api";

  let title = '';
  let author = '';
  let cover = '';
  let about = '';

  $: book = { title, author, cover, about };
  
  async function handleSubmit(event) {
    function getRandomInt(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    const newBook = {
      ...book,
      variation: getRandomInt(0, 2),
      favorite: false,
    }

    const { ok } = await httpPost('/', newBook);

    if (ok) {
      navigate('/');
    }
  }
</script>

<BackButtonRow />

<Header element="h1" size="large">Create</Header>

<form on:submit|preventDefault={handleSubmit}>
  <div class="fields">
    <TextInput label="Title" bind:value={title} />
    <TextInput label="Author" bind:value={author} />
    <TextInput label="Cover" bind:value={cover} />
    <TextInput label="Description" bind:value={about} multiline />
    <div>
      <Button>save</Button>
    </div>
  </div>

  <div>
    <Header>Preview</Header>
    <div class="preview">
      <BookCover {book} />
    </div>
  </div>
</form>

<style>
  form {
    display: grid;
    grid-auto-rows: auto;
    grid-template-columns: 1fr;
    gap: var(--spacingXLarge);
  }
  .fields {
    display: grid;
    grid-auto-rows: auto;
    gap: var(--spacingMedium);
  }
  .preview {
    display: grid;
    grid-template-columns: minmax(20vw, 10rem);
    grid-template-rows: minmax(32vw, 16rem);
  }
  @media (min-width: 48rem) {
    form {
      grid-template-columns: 60vw 20vw;
    }
  }
</style>