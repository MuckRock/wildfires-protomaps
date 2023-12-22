<script>
  import { onMount } from "svelte";
  import maplibregl from "maplibre-gl";
  import * as pmtiles from "pmtiles";
  import layers from "protomaps-themes-base";

  const protocol = new pmtiles.Protocol();

  maplibregl.addProtocol("pmtiles", protocol.tile);

  let container;
  let map;

  onMount(() => {
    map = new maplibregl.Map({
      container,
      style: {
        version: 8,
        glyphs:
          "https://protomaps.github.io/basemaps-assets/fonts/{fontstack}/{range}.pbf",
        sources: {
          protomaps: {
            type: "vector",
            url: "pmtiles://https://build.protomaps.com/20230918.pmtiles",
            attribution:
              '<a href="https://protomaps.com">Protomaps</a> © <a href="https://openstreetmap.org">OpenStreetMap</a>',
          },
        },
        layers: layers("protomaps", "dark"),
      },
      center: [-101.56660156843634, 39.27531259430637],
      zoom: 4,
      hash: true,
    });

    map.once("load", onLoad);
    map.on("moveend", onMoveEnd);

    // for debugging
    window["map"] = map;
  });

  function onLoad(e) {}
  function onMoveEnd(e) {}
</script>

<div bind:this={container} class="container">Map goes here</div>

<style>
  .container {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
  }
</style>
