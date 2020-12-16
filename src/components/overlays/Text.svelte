<script lang="ts">
  import { OverlayStyles } from "../../types";

  export let text: string;
  export let overlayStyles = new OverlayStyles()
    .setTop(50)
    .setLeft(100)
    .setOpacity(0.5);
</script>

<div
  class="wrapper"
  style={`top:${overlayStyles.top}px;left:${overlayStyles.left}px;opacity:${overlayStyles.opacity}`}>
  {text}
</div>

<style>
  .wrapper {
    background-color: red;
    color: white;
    font-size: 22px;
    padding: 0.5em;
    position: absolute;
  }
</style>
