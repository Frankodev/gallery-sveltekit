<script lang="ts">
import Card from '../components/Card.svelte';
import Spiner from '../components/Spiner.svelte';

let actualPage: number = 1

const getImages = async (page: number = 1) => {
  const response = await fetch(`https://api.unsplash.com/collections/?page=${page}&client_id=Hq8REJ6wAkDi7OP3kVmM4DV1O3yP5v_e-jxt4YbaxLc`);

  const data = await response.json();

  console.log(data);

  return data;
};

let promise =  getImages()

const handleNextPage = () => {
  actualPage ++;
  promise = getImages(actualPage)
}

const handlePrevPage = () => {
  actualPage --;
  promise = getImages(actualPage)
}

</script>


<svelte:head>
  <title>Gallery Svelte</title>
</svelte:head>

<section class="text-center">
  <h1>Gallery Svelte Kit</h1>
  <hr>

  {#await promise}
    <Spiner />
  {:then images} 
  <div class="row justify-content-evenly">
    {#each images as image}
    <div class="col-12 col-md-4 my-3 mx-1">
      <Card url={image.cover_photo.urls.thumb} tittle={image.title} download={image.cover_photo.urls.full} />
    </div>
    {/each}
  </div>

  {#if actualPage > 1}
  <button on:click={handlePrevPage} class="btn btn-outline-primary">&lt;</button>
  {/if}
  <button on:click={handleNextPage} class="btn btn-outline-primary">&gt;</button>
  {/await}

</section>