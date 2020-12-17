<script lang="ts">
  import shaka from "shaka-player/dist/shaka-player.compiled.js";
  import { onMount } from "svelte";
  import PlayBtn from "./PlayBtn.svelte";
  // import Text from "./components/overlays/Text.svelte";

  // export let textOverlay: string = "Test Text Overlay";

  let time = 0;
  let duration: number;
  let paused = true;
  let showControls = true;
  let showControlsTimeout;

  // let videoContainer: HTMLDivElement;
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

  function togglePlay() {
    paused ? videoComponent.play() : videoComponent.pause();
  }
</script>

<div class="video-container">
  <video
    width="640"
    class="shaka-video"
    bind:this={videoComponent}
    poster="//shaka-player-demo.appspot.com/assets/poster.jpg"
    on:click={togglePlay}
    bind:currentTime={time}
    bind:duration
    bind:paused><track kind="captions" />
  </video>
  <div class="controls">
    <div class="btn_play">
      <PlayBtn on:isPaused={togglePlay} />
    </div>
  </div>
</div>

<style>
  .video-container {
    position: relative;
    display: inline-block;
    border: 1px solid red;
  }

  .btn_play {
    height: 50px;
    width: 50px;
  }
</style>
