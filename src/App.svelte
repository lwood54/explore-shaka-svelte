<script lang="ts">
  import shaka from "shaka-player/dist/shaka-player.compiled.js";
  import { onMount } from "svelte";
  // import Text from "./components/overlays/Text.svelte";

  // export let textOverlay: string = "Test Text Overlay";

  let time = 0;
  let duration;
  let paused = true;

  let showControls = true;
  let showControlsTimeout;

  let videoContainer: HTMLDivElement;
  let videoComponent: HTMLVideoElement;

  onMount(async () => {
    const manifestUri =
      "https://storage.googleapis.com/shaka-demo-assets/angel-one/dash.mpd";
    const player = new shaka.Player(videoComponent);
    player.addEventListener("error", (e) => console.error(e));

    // load manifest
    try {
      await player.load(manifestUri);
    } catch (e) {
      console.error("Webplayer failed to load ", e);
    }
  });

  function handleMousemove(e) {
    // Make the controls visible, but fade out after
    // 2.5 seconds of inactivity
    clearTimeout(showControlsTimeout);
    showControlsTimeout = setTimeout(() => (showControls = false), 2500);
    showControls = true;

    if (!(e.buttons & 1)) return; // mouse not down
    if (!duration) return; // video not loaded yet

    const { left, right } = this.getBoundingClientRect();
    time = (duration * (e.clientX - left)) / (right - left);
  }

  function handleMousedown(e) {
    // we can't rely on the built-in click event, because it fires
    // after a drag — we have to listen for clicks ourselves

    function handleMouseup() {
      if (paused) e.target.play();
      else e.target.pause();
      cancel();
    }

    function cancel() {
      e.target.removeEventListener("mouseup", handleMouseup);
    }

    e.target.addEventListener("mouseup", handleMouseup);

    setTimeout(cancel, 200);
  }

  function format(seconds) {
    if (isNaN(seconds)) return "...";

    const minutes = Math.floor(seconds / 60);
    seconds = Math.floor(seconds % 60);
    if (seconds < 10) seconds = "0" + seconds;

    return `${minutes}:${seconds}`;
  }
</script>

<!-- svelte-ignore a11y-media-has-caption -->
<div class="video-container" bind:this={videoContainer}>
  <video
    width="640"
    class="shaka-video"
    bind:this={videoComponent}
    poster="//shaka-player-demo.appspot.com/assets/poster.jpg"
    on:mousemove={handleMousemove}
    on:mousedown={handleMousedown}
    bind:currentTime={time}
    bind:duration
    bind:paused />
  <!-- {#if textOverlay}
    <Text text={textOverlay} />
  {/if} -->
</div>

<style>
  .video-container {
    position: relative;
    display: inline-block;
  }
</style>
