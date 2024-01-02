<script>
  import { onMount } from "svelte";
  import maplibregl from "maplibre-gl";
  import * as pmtiles from "pmtiles";
  import layers from "protomaps-themes-base";

  const protocol = new pmtiles.Protocol();

  maplibregl.addProtocol("pmtiles", protocol.tile);

  let container;
  let map;

  const decades = [1980, 1990, 2000, 2010, 2020];
  const colors = ["#ffffb2", "#fecc5c", "#fd8d3c", "#e31a1c", "#e31a1c"];

  const scale = decades.reduce((m, d, i) => {
    m = [...m, ["==", ["get", "decade"], d], colors[i]];
    return m;
  }, []);

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

    // for debugging
    window["map"] = map;
  });

  function onLoad(e) {
    const map = e.target;

    const firstSymbolLayer = map
      .getStyle()
      .layers.find((layer) => layer.type === "symbol");

    map.addSource("fires", {
      type: "vector",
      url: "pmtiles://" + new URL("fires.pmtiles", window.location.href),
    });

    map.addLayer(
      {
        id: "fires-fill",
        source: "fires",
        "source-layer": "all-years",
        type: "fill",
        layout: {},
        paint: {
          "fill-color": ["case", ...scale, "#ddd"],
          "fill-opacity": 0.5,
        },
      },
      firstSymbolLayer.id
    );

    map.addLayer({
      id: "fires-lines",
      source: "fires",
      "source-layer": "all-years",
      type: "line",
      paint: {
        "line-color": "#fff",
        "line-opacity": 0.5,
        "line-width": [
          "interpolate",
          ["linear", 0.5],
          ["zoom"],
          6,
          0,
          16,
          0.75,
        ],
      },
    });
  }
</script>

<div bind:this={container} class="container">
  <noscript> This experience requires JavaScript to function. </noscript>
</div>

<style>
  .container {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
  }
</style>
