<script lang="ts">
  import maplibregl, { Map, type LngLatLike } from "maplibre-gl";
  import { onMount } from "svelte";
  import esriStyle from "../esriStyle";

  let { center }: { center: LngLatLike } = $props();
  let mapContainer: HTMLElement;
  let map: Map;

  export function zoomOut() {
    map.zoomOut();
  }
  export function getZoom() {
    return map.getZoom();
  }

  onMount(() => {
    map = new maplibregl.Map({
      container: mapContainer,
      style: esriStyle,
      center: center,
      zoom: 6,
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
