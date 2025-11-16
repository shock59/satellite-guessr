<script lang="ts">
  import { onMount } from "svelte";
  import { randomPoint } from "@turf/random";
  import pointsWithinPolygon from "@turf/points-within-polygon";
  import land from "./land";
  import type { Position } from "geojson";
  import GuessMap from "./maps/GuessMap.svelte";
  import PreviewWindow from "./PreviewWindow.svelte";

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
    previewMapPoint = randomLandPoint().geometry.coordinates;
  });
</script>

<main>
  <GuessMap />

  {#if previewMapPoint != undefined}
    <PreviewWindow center={previewMapPoint as [number, number]} />
  {/if}
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    background: #1b1b1b;
  }
</style>
