<script lang="ts">
  import maplibregl from "maplibre-gl";
  import { onMount } from "svelte";
  import sentinelStyle from "./sentinelStyle";

  let mapContainer: HTMLElement;

  onMount(() => {
    const map = new maplibregl.Map({
      container: mapContainer,
      style: sentinelStyle,
      center: [0, 0],
      zoom: 1,
      attributionControl: {
        compact: false,
        customAttribution: '<a href="https://maplibre.org/">MapLibre</a>',
      },
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
