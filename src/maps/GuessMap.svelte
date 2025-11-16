<script lang="ts">
  import maplibregl, { Marker } from "maplibre-gl";
  import { onMount } from "svelte";
  import esriStyle from "../esriStyle";

  let mapContainer: HTMLElement;

  onMount(() => {
    const map = new maplibregl.Map({
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
        guessMarker = new Marker().setLngLat(event.lngLat).addTo(map);
        guessMarker.getElement().style.cursor = "pointer";
      } else {
        guessMarker.setLngLat(event.lngLat);
      }
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
