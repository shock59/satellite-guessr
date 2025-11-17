<script lang="ts">
  import { onMount } from "svelte";
  import { randomPoint } from "@turf/random";
  import pointsWithinPolygon from "@turf/points-within-polygon";
  import land from "./land";
  import type { Position } from "geojson";
  import GuessMap from "./maps/GuessMap.svelte";
  import PreviewWindow from "./PreviewWindow.svelte";
  import type { LngLatLike } from "maplibre-gl";

  let answerPoint: Position | undefined = $state();
  let guessPosition: LngLatLike | undefined = $state();
  let guessed: boolean = $state(false);

  let guessMap: GuessMap;

  function randomLandPoint() {
    const point = randomPoint().features[0];
    if (pointsWithinPolygon(point, land).features.length > 0) {
      return point;
    } else {
      return randomLandPoint();
    }
  }

  function updateGuessPosition(newPosition: LngLatLike) {
    guessPosition = newPosition;
  }

  function submitGuess() {
    guessed = true;
    guessMap.showAnswer(guessPosition!, answerPoint as [number, number]);
  }

  onMount(() => {
    answerPoint = randomLandPoint().geometry.coordinates;
  });
</script>

<main>
  <GuessMap bind:this={guessMap} {updateGuessPosition} />

  {#if !guessed && answerPoint != undefined}
    <PreviewWindow
      center={answerPoint as [number, number]}
      guessPositionSet={guessPosition != undefined}
      {submitGuess}
    />
  {/if}
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    background: #1b1b1b;
  }
</style>
