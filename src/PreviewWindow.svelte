<script lang="ts">
  import type { LngLatLike } from "maplibre-gl";
  import PreviewMap from "./maps/PreviewMap.svelte";

  let {
    center,
    guessPositionSet,
    submitGuess,
  }: {
    center: LngLatLike;
    guessPositionSet: boolean;
    submitGuess: () => unknown;
  } = $props();
</script>

<div id="preview-window">
  <div id="header">Find this location</div>
  <PreviewMap {center} />
  <div id="footer">
    <button onclick={submitGuess} disabled={!guessPositionSet}>Guess</button>
  </div>
</div>

<style>
  #preview-window {
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
