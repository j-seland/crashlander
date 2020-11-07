<script>
  import { onMount } from 'svelte';

  export let fuelquantity;

  const LOWFUELWARNING = 40;

  onMount(() => {
    document.getElementById('fuel').disabled = true
  });
</script>

<style>
/* Chrome */
@media screen and (-webkit-min-device-pixel-ratio:0) {
  input[type='range'] {
    overflow: hidden;
    width: 160px;
    -webkit-appearance: none;
    background-color: #ff0000;
  }

  input[type='range']::-webkit-slider-runnable-track {
    height: 10px;
    -webkit-appearance: none;
    color: #006600;
    margin-top: -1px;
  }

  input[type='range']::-webkit-slider-thumb {
    width: 6px;
    -webkit-appearance: none;
    height: 12px;
    cursor: ew-resize;
    background: #ffff00;
    box-shadow: -160px 0 0 160px #006600;
  }
}
/* FF */
input[type="range"]::-moz-range-progress {
  background-color: #006600;
}

input[type="range"]::-moz-range-track {
  background-color: #ff0000;
}
/* IE */
input[type="range"]::-ms-fill-lower {
  background-color: #006600;
}

input[type="range"]::-ms-fill-upper {
  background-color: #ff0000;
}
</style>

<input type="range" id="fuel" name="fuel" min="0" max="150" bind:value={fuelquantity}>
{#if fuelquantity < LOWFUELWARNING}
  <audio preload autoplay>
    <source src="./audio/alarm2.mp3" type="audio/mpeg">
  </audio>
{/if}
