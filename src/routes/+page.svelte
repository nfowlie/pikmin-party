<script>
  import "@picocss/pico";
  import pikmin from "$lib/assets/Pikmin-red.webp";
  import pikminSound from "$lib/assets/Pikmin sound effect-(p).flac";
  import { onMount } from "svelte";

  let playing = false;

  let soundEffect;
  onMount(() => {
    soundEffect = new Audio(pikminSound);
  });

  const min = 1,
    max = 600;

  let soundEffectTimeout;
  let countdownInterval;

  const handlePlayPause = () => {
    if (playing) {
      clearTimeout(soundEffectTimeout);
      clearInterval(countdownInterval);
      document.querySelector("#from_pause_to_play").beginElement();
    } else {
      setRandomTimeout();
      document.querySelector("#from_play_to_pause").beginElement();
    }

    playing = !playing;
  };

  const setRandomTimeout = () => {
    let countdown = (Math.floor(Math.random() * (max - min)) + min) * 1000;
    let increment = 0;

    countdownInterval = setInterval(() => {
      increment += 1000;
      console.log((countdown - increment) / 1000);
    }, 1000);
    soundEffectTimeout = setTimeout(() => {
      clearInterval(countdownInterval);
      soundEffect.play();
      setRandomTimeout();
    }, countdown);
  };
</script>

<div id="pikmin">
  <h1>Welcome To Pikmin Party!</h1>
  <p>Click the play button to play a Pikmin sound on a random interval</p>
  <img src={pikmin} alt="Pikmin" height="374" width="344" />
  <button on:click={() => handlePlayPause()}
    ><svg width="104" height="104" id="pause">
      <line
        id="line1"
        x1="38"
        y1="30"
        x2="38"
        y2="70"
        style="stroke-width:4px;stroke:white;stroke-linecap: round;"
      />
      <path
        id="line2"
        d="M 38 30 L 70 50 L 38 70"
        rx="10"
        ry="10"
        style="stroke-width:4px;stroke:white;fill:white;stroke-linejoin: round;stroke-linecap: round;"
      >
        <animate
          attributeName="d"
          dur="300ms"
          from="M 66 30 L 66 50 L 66 70"
          to="M 38 30 L 70 50 L 38 70"
          begin="indefinite"
          fill="freeze"
          id="from_pause_to_play"
        />
      </path>
      <animate
        xlink:href="#line2"
        attributeName="d"
        dur="300ms"
        from="M 38 30 L 70 50 L 38 70"
        to="M 66 30 L 66 50 L 66 70"
        fill="freeze"
        id="from_play_to_pause"
        begin="indefinite"
      /></svg
    ></button
  >
</div>

<style>
  #pikmin {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  button {
    width: 344px;
  }
</style>
