<script lang="ts">
  import maplibregl, { type LngLatLike } from "maplibre-gl";
  import { onMount } from "svelte";
  import esriStyle from "../esriStyle";

  let { center }: { center: LngLatLike } = $props();
  let mapContainer: HTMLElement;

  onMount(() => {
    const map = new maplibregl.Map({
      container: mapContainer,
      style: esriStyle,
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
    width: 100%;
    height: 256px;

    border-top: var(--border-style);
    border-bottom: var(--border-style);
  }
</style>
