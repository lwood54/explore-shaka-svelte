<script>
  import { createEventDispatcher } from "svelte";

  let isPaused = true;
  const pause =
    "M11,10 L18,13.74 18,22.28 11,26 M18,13.74 L26,18 26,18 18,22.28";
  const play = "M11,10 L17,10 17,26 11,26 M20,10 L26,10 26,26 20,26";
  let fromStr = pause;
  let toStr = play;
  let animation;

  const dispatch = createEventDispatcher();

  function handleClick() {
    fromStr = isPaused ? pause : play;
    toStr = isPaused ? play : pause;
    isPaused = !isPaused;
    animation.beginElement();
    dispatch("isPaused", isPaused);
  }
</script>

<div>
  <button
    on:click={handleClick}
    class="ytp-play-button ytp-button"
    aria-live="assertive"
    tabindex="32"
    aria-label="Pause">
    <svg
      width="100%"
      height="100%"
      viewBox="0 0 36 36"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink">
      <defs>
        <path
          id="ytp-12"
          d="M11,10 L18,13.74 18,22.28 11,26 M18,13.74 L26,18 26,18 18,22.28">
          <animate
            bind:this={animation}
            begin="indefinite"
            attributeType="XML"
            attributeName="d"
            fill="freeze"
            from={fromStr}
            to={toStr}
            dur="0.2s"
            keySplines=".4 0 1 1"
            repeatCount="1" />
        </path>
      </defs>
      <use xlink:href="#ytp-12" class="ytp-svg-shadow" />
      <use xlink:href="#ytp-12" class="ytp-svg-fill" />
    </svg>
  </button>
</div>

<style>
  @import url(https://fonts.googleapis.com/css?family=Roboto:400,300,100);

  button {
    outline: none;
    border: 0px solid;
    background: transparent;
  }

  .ytp-play-button {
    fill: black;
    opacity: 0.85;
  }

  .ytp-play-button:hover {
    cursor: pointer;
    opacity: 1;
  }
</style>
