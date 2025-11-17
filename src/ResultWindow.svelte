<script lang="ts">
  import distance from "@turf/distance";
  import { LngLat, type LngLatLike } from "maplibre-gl";
  import { onMount } from "svelte";

  let {
    guessPosition,
    answerPoint,
  }: {
    guessPosition: LngLatLike;
    answerPoint: LngLatLike;
  } = $props();

  let distanceKm: string = $state("");
  let distanceMiles: string = $state("");

  onMount(() => {
    distanceKm = distance(
      LngLat.convert(guessPosition).toArray(),
      LngLat.convert(answerPoint).toArray()
    ).toFixed(1);

    distanceMiles = distance(
      LngLat.convert(guessPosition).toArray(),
      LngLat.convert(answerPoint).toArray(),
      { units: "miles" }
    ).toFixed(1);
  });
</script>

<div id="result-window">
  <div id="header">
    Your guess was {distanceKm}km ({distanceMiles} miles) away
  </div>
  <div id="footer">
    <button onclick={() => location.reload()}>Play again</button>
  </div>
</div>

<style>
  #result-window {
    width: 256px;

    position: absolute;
    top: 32px;
    left: 32px;

    color: #e0e0e0;
    background: #1b1b1b;

    font-size: 16px;

    --border-style: 1px solid #000000;
    border: var(--border-style);
    border-radius: 8px;

    box-shadow: 8px 8px 8px #1b1b1bab;
  }

  #header,
  #footer {
    padding: 8px 16px;

    text-align: center;
    font-weight: bold;
  }

  #footer button {
    padding: 6px 32px;

    color: inherit;
    background: #2a6b31;

    font: inherit;

    border: none;
    border-radius: 8px;
    outline: none;

    cursor: pointer;

    transition: 200ms;
  }

  #footer button:hover:not(:disabled) {
    background: #328a3b;
  }

  #footer button:disabled {
    cursor: not-allowed;
    opacity: 0.6;
  }
</style>
