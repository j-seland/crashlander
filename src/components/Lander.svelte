<script>
  import { fade } from 'svelte/transition';

  export let direction;
</script>

<style>
.rocket {
  text-align: center;
  padding: 20px;
}

.clr-up {
  font-size: 2rem;
  color: #ff0000;
}

.clr-down {
  font-size: 2rem;
  color: #00ff00;
}
</style>

<div class="rocket">
  <div>
    {#if direction}
      <span class="clr-down">⬇️⬇️⬇️ DESCENDING ⬇️⬇️⬇️</span>
    {:else}
      <span class="clr-up">⬆️⬆️⬆️ ASCENDING ⬆️⬆️⬆️</span>
      <audio preload autoplay>
        <source src="./audio/alarm1.mp3" type="audio/mpeg">
      </audio>
    {/if}
  </div>
  <img transition:fade="{{ duration: 2000 }}" src="./images/lunarlander.jpg" width="320" height="320" alt="Lunar Lander">
</div>
