<script lang="ts">
import Card from '../components/Card.svelte';
import Spiner from '../components/Spiner.svelte';

  let ref: any;

  let actualPage: number = 1;
  let searchParam: string = "";

const getImages = async (page: number = 1, searchParam = "beach") => {

  if(searchParam.trim() == "") {
    return
  }

  const response = await fetch(`https://api.unsplash.com/search/collections/?query=${searchParam}&page=${page}&client_id=Hq8REJ6wAkDi7OP3kVmM4DV1O3yP5v_e-jxt4YbaxLc`);

  const data = await response.json();

  console.log(data);

  return data;
};

let promise = getImages();

const handleSubmit = (e: any) => {
  actualPage = 1;
  promise = getImages(actualPage, searchParam);
  // searchParam = "";
}

const handleNextPage = () => {
  actualPage ++;
  promise = getImages(actualPage, searchParam)
}

const handlePrevPage = () => {
  actualPage --;
  promise = getImages(actualPage, searchParam)
}
</script>


<svelte:head>
  <title>Search</title>
</svelte:head>

<form on:submit|preventDefault={handleSubmit} class="form-group d-flex gap-4 align-items-center pt-4">
  <label for="" class="fw-bold">Gallery</label>
  <input bind:value={searchParam} bind:this={ref} type="text" placeholder="Search photography" class="form-control" on:focus={() => ref.select()}>
  <button type="submit" class="btn btn-primary">Search</button>
</form>

<section class="text-center">

  {#await promise}
    <Spiner />
  {:then images} 
  <div class="row justify-content-evenly">
    {#each images.results as image}
    <div class="col-12 col-md-4 my-3 mx-1">
      <Card url={image.cover_photo.urls.thumb} tittle={image.title} download={image.cover_photo.urls.full} />
    </div>
    {/each}
  </div>

  {#if actualPage > 1}
  <button on:click={handlePrevPage} class="btn btn-outline-primary">&lt;</button>
  {/if}
  <button on:click={handleNextPage} class="btn btn-outline-primary">&gt;</button>

  {:catch}
    <h3>Error</h3>
  {/await}
  
</section>