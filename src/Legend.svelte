<script>
  import { onMount, onDestroy } from "svelte";

  export let map;
  export let position = "top-right";
  export let dark = false;

  let container;

  const control = {
    onAdd() {
      return container;
    },
    onRemove() {},
  };

  onMount(() => {
    map.addControl(control, position);
  });

  onDestroy(() => {
    map.removeControl(control);
  });
</script>

<div bind:this={container} class="legend" class:dark>
  <slot />
</div>

<style>
  .legend {
    background-color: white;
    border-radius: 5px;
    margin: 2em;
    max-width: 200px;
    padding: 1em;
    pointer-events: all;
    position: relative;
    z-index: 10;
  }

  .legend.dark {
    background-color: black;
  }

  .legend.dark :global(*) {
    color: #fff;
  }
</style>
