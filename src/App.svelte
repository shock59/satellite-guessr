<script lang="ts">
  import maplibregl from "maplibre-gl";
  import { onMount } from "svelte";
  import sentinelStyle from "./sentinelStyle";
  import { randomPoint } from "@turf/random";
  import pointsWithinPolygon from "@turf/points-within-polygon";
  import land from "./land";
  import type { Position } from "geojson";
  import PreviewMap from "./PreviewMap.svelte";

  let mapContainer: HTMLElement;
  let previewMapPoint: Position | undefined = $state();

  function randomLandPoint() {
    const point = randomPoint().features[0];
    if (pointsWithinPolygon(point, land).features.length > 0) {
      return point;
    } else {
      return randomLandPoint();
    }
  }

  onMount(() => {
    const map = new maplibregl.Map({
      container: mapContainer,
      style: sentinelStyle,
      center: [0, 0],
      zoom: 1,
      attributionControl: {
        compact: false,
        customAttribution:
          '<a href="https://maplibre.org/">MapLibre</a> | Map tiles &copy; <a href="https://www.esri.com/">Esri</a> | Images from Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community',
      },
    });
    previewMapPoint = randomLandPoint().geometry.coordinates;
  });
</script>

<main>
  <div bind:this={mapContainer} id="map-container"></div>

  {#if previewMapPoint != undefined}
    <PreviewMap center={previewMapPoint as [number, number]} />
  {/if}
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
