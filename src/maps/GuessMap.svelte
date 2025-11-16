<script lang="ts">
  import maplibregl from "maplibre-gl";
  import { onMount } from "svelte";
  import esriStyle from "../esriStyle";

  let mapContainer: HTMLElement;

  onMount(() => {
    new maplibregl.Map({
      container: mapContainer,
      style: esriStyle,
      center: [0, 0],
      zoom: 1,
      maxZoom: 19,
      attributionControl: {
        compact: false,
        customAttribution:
          '<a href="https://maplibre.org/">MapLibre</a> | Map tiles &copy; <a href="https://www.esri.com/">Esri</a> | Images from Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community',
      },
    });
  });
</script>

<div bind:this={mapContainer} id="map-container"></div>

<style>
  #map-container {
    width: 100%;
    height: 100%;
  }
</style>
