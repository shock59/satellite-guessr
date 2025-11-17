<script lang="ts">
  import maplibregl, {
    LngLat,
    Map,
    Marker,
    type LngLatLike,
  } from "maplibre-gl";
  import { onMount } from "svelte";
  import esriStyle from "../esriStyle";
  import type { Feature, LineString } from "geojson";
  import bbox from "@turf/bbox";

  let {
    updateGuessPosition,
  }: {
    updateGuessPosition: (newPosition: LngLatLike) => unknown;
  } = $props();

  let map: Map;
  let mapContainer: HTMLElement;

  export function showAnswer(
    guessPosition: LngLatLike,
    answerPoint: LngLatLike
  ) {
    new Marker({ color: "#48a248", scale: 1.2 })
      .setLngLat(answerPoint)
      .addTo(map);

    const line: Feature<LineString> = {
      type: "Feature",
      properties: {},
      geometry: {
        type: "LineString",
        coordinates: [
          LngLat.convert(guessPosition).toArray(),
          LngLat.convert(answerPoint).toArray(),
        ],
      },
    };
    map.addSource("answerLine", {
      type: "geojson",
      data: line,
    });
    map.addLayer({
      id: "answerLine",
      type: "line",
      source: "answerLine",
      layout: {
        "line-join": "round",
        "line-cap": "round",
      },
      paint: {
        "line-color": "#e24848",
        "line-width": 8,
      },
    });

    map.fitBounds(bbox(line) as [number, number, number, number], {
      padding: 128,
    });
  }

  onMount(() => {
    map = new maplibregl.Map({
      container: mapContainer,
      style: esriStyle,
      center: [0, 0],
      zoom: 2,
      maxZoom: 19,
      attributionControl: {
        compact: false,
        customAttribution:
          '<a href="https://maplibre.org/">MapLibre</a> | Map tiles &copy; <a href="https://www.esri.com/">Esri</a> | Images from Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community',
      },
    });

    map.getCanvas().style.cursor = "pointer";
    map.on("mouseup", () => {
      map.getCanvas().style.cursor = "pointer";
    });
    map.on("mousedown", () => {
      map.getCanvas().style.cursor = "";
    });

    let guessMarker: Marker | undefined = undefined;
    map.on("click", (event) => {
      if (!guessMarker) {
        guessMarker = new Marker({ color: "#e24848", scale: 1.2 })
          .setLngLat(event.lngLat)
          .addTo(map);
        guessMarker.getElement().style.cursor = "pointer";
      } else {
        guessMarker.setLngLat(event.lngLat);
      }
      updateGuessPosition(event.lngLat);
    });

    map.getContainer().style.fontFamily = "inherit";
  });
</script>

<div bind:this={mapContainer} id="map-container"></div>

<style>
  #map-container {
    width: 100%;
    height: 100%;
  }
</style>
