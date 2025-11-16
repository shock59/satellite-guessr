<script lang="ts">
  import maplibregl, { type LngLatLike } from "maplibre-gl";
  import { onMount } from "svelte";
  import sentinelStyle from "./sentinelStyle";

  let { center }: { center: LngLatLike } = $props();
  let mapContainer: HTMLElement;

  onMount(() => {
    const map = new maplibregl.Map({
      container: mapContainer,
      style: sentinelStyle,
      center: center,
      zoom: 5,
      attributionControl: {},
      interactive: false,
    });
  });
</script>

<div bind:this={mapContainer} id="map-container"></div>

<style>
  #map-container {
    width: 256px;
    height: 256px;

    position: absolute;
    top: 32px;
    left: 32px;

    border: 1px solid black;
  }
</style>
