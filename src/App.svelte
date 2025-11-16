<script lang="ts">
  import maplibregl, { Marker } from "maplibre-gl";
  import { onMount } from "svelte";
  import sentinelStyle from "./sentinelStyle";
  import { randomPoint } from "@turf/random";
  import pointsWithinPolygon from "@turf/points-within-polygon";
  import land from "./land";

  let mapContainer: HTMLElement;

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
        customAttribution: '<a href="https://maplibre.org/">MapLibre</a>',
      },
    });

    const point = randomLandPoint().geometry.coordinates;
    new Marker({ color: "#ff0000" })
      .setLngLat(point as [number, number])
      .addTo(map);
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
