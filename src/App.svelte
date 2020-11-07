<script>
  import { fly } from 'svelte/transition';
  import Intro from './components/Intro.svelte';
  import Footer from './components/Footer.svelte';
  import Fuelstatus from './components/Fuelstatus.svelte';
  import Lander from './components/Lander.svelte';

	const RocketStatus = ['Waiting','Landing','Fuel Out','Crashed','Successful Touchdown'];
  const StatusColor  = ['#65dddd','#65afff','#ffa000','#ff0000','#00ff00'];

  const DOWN = true;
  const UP = false;

  const Waiting = 0;
  const Landing = 1;
	const Fuelout = 2;
	const Crashed = 3;
	const Success = 4;

  const Zfactor = 5;
  const Height = 500;
  const Fuelquantity = 150;
  const Fuelconsumption = 0;

  let speedColor = '#00ff00';
  let yes = false;
  let direction = DOWN;

  let rocket_landing = true,
	    rocket_status = Waiting,
      rocket_zfactor = Zfactor,
      rocket_height = Height,
	    rocket_fuelquantity = Fuelquantity,
      rocket_fuelconsumption = Fuelconsumption,
	    rocket_passes = 0,
		  rocket_speed = 0;

  const systemReset = () => {
    rocket_landing = true;
    rocket_status = Landing;
    rocket_zfactor = Zfactor;
    rocket_height = Height;
    rocket_fuelquantity = Fuelquantity;
    rocket_fuelconsumption = Fuelconsumption;
    rocket_passes = 0;
	  rocket_speed = 0;
  }

  const fuelCheck = () => {
    if (rocket_fuelconsumption > rocket_fuelquantity) {
      rocket_fuelconsumption = rocket_fuelquantity;
    }
    rocket_fuelquantity = rocket_fuelquantity - rocket_fuelconsumption;

    if (rocket_fuelquantity < Zfactor) {
      rocket_height = 0;
      rocket_fuelconsumption = 0;
      rocket_landing = false;
      rocket_status = Fuelout;
    }
  }

  const getHeight = () => {
    rocket_height = rocket_height + rocket_speed + rocket_zfactor % 2;

    if (rocket_height > Height) rocket_height = Height;

    if ((rocket_height < 0 || rocket_height < Zfactor) && (rocket_fuelquantity < 0 || Math.abs(rocket_speed) > Zfactor)) {
      rocket_height = 0;
      rocket_fuelconsumption = 0;
      rocket_landing = false;
	    rocket_status = Crashed;
		}
  }

  const getZfactor = () => {
    return rocket_fuelconsumption - Zfactor;
  }

  const engageThruster = () => {
		rocket_passes++;

    if (rocket_landing && rocket_passes > 0) rocket_status = Landing;

    rocket_zfactor = getZfactor()

    fuelCheck();

    if (rocket_landing) {
      getHeight();
    }

    if (rocket_landing) {

      rocket_speed = rocket_speed + rocket_zfactor;

      if (rocket_speed < 0) {
        direction = DOWN
      }	else {
        direction = UP
      }
      if (Math.abs(rocket_speed) > Zfactor) {
        speedColor = '#ff0000';
      } else {
        speedColor = '#00ff00';
      }

      if ((Math.abs(rocket_height) === 0 || Math.abs(rocket_height) < Zfactor) && Math.abs(rocket_speed) <= Zfactor) {
        rocket_height = 0;
        rocket_speed = 0;
        rocket_fuelconsumption = 0;
        rocket_landing = false;
		    rocket_status = Success;
			}
    }
	}
</script>

<style>
.rocket-status {
	font-size: 1.5rem;
	color: var(--clr-yellow);
  margin: 10px;
}

.rocket-control {
  text-align: center;
	font-size: 1.5rem;
	color: var(--clr-green);
  margin-top: 40px;
}

.mbox1 {
  background-color: var(--clr-black);
  border: 2px solid var(--clr-border);
  padding: 15px;
}

.mbox2 {
  background-color: var(--clr-primary);
  border: 2px solid var(--clr-border);
  padding: 15px;
}

.mbox3 {
  font-size: 1.5rem;
  background-color: var(--clr-black);
	color: var(--clr-yellow);
  border: 2px solid var(--clr-border);
  padding: 15px;
  text-align: center;
}

span {
  color: var(--speed-color);
}

.grid-container {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
  margin: 0 20px;
}

@media only screen and (min-width: 992px) {
  .grid-container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 1rem;
    margin: 0 20px;
  }
}
</style>

<main>
  <Intro />

  <div class="grid-container">

    <div class="mbox1">
      <div class="rocket-status">
        <p>Current status: <span style="color: {StatusColor[rocket_status]}">{RocketStatus[rocket_status]}</span></p>
        <p>Height: {rocket_height} m</p>
        <p>Fuelquantity: {rocket_fuelquantity} liter</p>
        <Fuelstatus fuelquantity={rocket_fuelquantity} />
        <hr>
        <p>Speed: <span style="--speed-color: {speedColor}">{Math.abs(rocket_speed)}</span> m/s</p>
        <hr>
        <p>Passes: {rocket_passes}</p>
        <hr>
      </div>
    </div>

    <div class="mbox2">
      <div class="rocket-control">
        Thrusterfuel:&nbsp;<input type="number" min="0" max="50" bind:value={rocket_fuelconsumption}>
        <div style="margin-top:10px;">
          <button on:click|once={engageThruster}>System Go</button>
          <button on:click={systemReset}>System Reset</button>
          <button on:click={engageThruster}>Engage Thruster</button>
        </div>
        <label>
          Show Z-factor:
        	<input type=checkbox bind:checked={yes}>
        </label>
        {#if yes}
          <p transition:fly="{{ x: -200, duration: 1000 }}">Z-factor: {rocket_zfactor}</p>
        {:else}
          <p transition:fly="{{ x: 200, duration: 1000 }}">Z-factor: 👽</p>
        {/if}
      </div>
    </div>

    <div class="mbox3">
      {#if rocket_status > Waiting}
        <Lander {direction} />
      {/if}
    </div>

  </div>
</main>
<Footer />
