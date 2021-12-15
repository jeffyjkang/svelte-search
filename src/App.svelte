<script lang="ts">
  let search: string;
  let loading: boolean = false;
  let gifs = [];

  const api_key: string = `&api_key=${API_KEY}&limit=15`;
  const api_url = 'https://api.giphy.com/v1/gifs/search?q=';

  const submitForm = async () => {
    loading = true;
    gifs = [];
    const url = `${api_url}${search}${api_key}`;
    try {
      let res = await fetch(url);
      if (!res.ok) {
        throw new Error(`HTTP error, status: ${res.status}`);
      }
      let data = await res.json();
      gifs = data.data.map((gif: any)=>gif.images.fixed_height.url);
      loading = false;
    }
    catch(e) {
      console.log(e);
    }
  }
</script>

<form on:submit|preventDefault={submitForm}>
  <label for='search'>Search</label>
  <input id='search' name='search' bind:value={search} />
  <button type='submit'>Submit</button>
</form>
{#if loading}
  <p>...loading</p>
{/if}
<div class='resContainer'>
  {#each gifs as gif, i}
    <img src={gif}  alt={`${i}-gif`}/>
  {/each}
</div>

<style>
  .resContainer {
    border: 1px dotted gray;
    column-count: 3;
  }
  img {
    width: 100%;
    height: auto;
  }
</style>

