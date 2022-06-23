<script lang="ts">
  export let url: string, tittle: string, download: string;
</script>

<div class="card m-auto" style="width: 18rem;">
  <img src={url} class="card-img-top" alt={tittle}>
  <div class="card-body">
   <h5 class="card-title">{tittle}</h5>
    <a href={download} target="_blanc" class="btn btn-primary">Open image</a>
  </div>
</div>