<script lang="ts">
  import { onMount } from "svelte";
  import { randomPoint } from "@turf/random";
  import pointsWithinPolygon from "@turf/points-within-polygon";
  import land from "./land";
  import type { Position } from "geojson";
  import GuessMap from "./maps/GuessMap.svelte";
  import PreviewWindow from "./PreviewWindow.svelte";
  import type { LngLatLike } from "maplibre-gl";
  import ResultWindow from "./ResultWindow.svelte";

  let answerPoint: [number, number] | undefined = $state();
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
    guessMap.showAnswer(guessPosition!, answerPoint!);
  }

  onMount(() => {
    answerPoint = randomLandPoint().geometry.coordinates as [number, number];
  });
</script>

<main>
  <GuessMap bind:this={guessMap} {updateGuessPosition} />

  {#if !guessed && answerPoint != undefined}
    <PreviewWindow
      center={answerPoint}
      guessPositionSet={guessPosition != undefined}
      {submitGuess}
    />
  {:else if guessed && guessPosition && answerPoint}
    <ResultWindow {guessPosition} {answerPoint} />
  {/if}
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    background: #1b1b1b;
  }
</style>
