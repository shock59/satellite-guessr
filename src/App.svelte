<script lang="ts">
  import maplibregl from "maplibre-gl";
  import { onMount } from "svelte";

  let mapContainer: HTMLElement;

  onMount(() => {
    const map = new maplibregl.Map({
      container: mapContainer,
      style: "https://demotiles.maplibre.org/globe.json",
      center: [0, 0],
      zoom: 1,
    });
  });
</script>

<main>
  <div bind:this={mapContainer} id="map-container"></div>
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
  }

  #map-container {
    width: 100%;
    height: 100%;
  }
</style>
