<script lang="ts">
  import { onMount } from "svelte";

  const manifestUri =
    "https://storage.googleapis.com/shaka-demo-assets/angel-one/dash.mpd";

  onMount(async () => {
    // When using the UI, the player is made automatically by the UI object.
    const video = document.getElementById("video");
    const ui = video["ui"];
    console.log("ui: ", ui);
    const controls = ui.getControls();
    const player = controls.getPlayer();

    // Listen for error events.
    // player.addEventListener('error', onPlayerErrorEvent);
    // controls.addEventListener('error', onUIErrorEvent);

    // Try to load a manifest.
    // This is an asynchronous process.
    try {
      await player.load(manifestUri);
      // This runs if the asynchronous load is successful.
      console.log("The video has now been loaded!");
    } catch (error) {
      console.log("Error: ", error);
      // onPlayerError(error);
    }
  });
</script>

<div
  data-shaka-player-container
  style="max-width:40em"
  data-shaka-player-cast-receiver-id="930DEB06">
  <!-- The data-shaka-player tag will make the UI library use this video element.
   If no video is provided, the UI will automatically make one inside the container div. -->
  <video
    autoplay
    data-shaka-player
    id="video"
    style="width:100%;height:100%"><track kind="captions" /></video>
</div>
