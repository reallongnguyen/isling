<script>
  const layers = [0, 1, 2, 3, 4, 5, 6, 7, 8];

  let lastLayerHeight;
  let lastLayerOffsetHeight;
  let scrollY;
</script>

<style>
  .parallax-container {
    position: fixed;
    width: 2400px;
    height: 712px;
    left: 50%;
    transform: translate(-50%, 0);
  }

  .parallax-container img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    will-change: transform;
  }

  .parallax-container img:last-child::after {
    content: "";
    position: absolute;
    width: 100%;
    height: 100%;
    background: rgb(45, 10, 13);
  }

  .content {
    position: relative;
    width: 100%;
    min-height: 100vh;
    color: rgb(220, 113, 43);
    padding: 4em 0.5em 0.5em 0.5em;
    box-sizing: border-box;
    pointer-events: none;
  }

  .foreground {
    position: absolute;
    top: 711px;
    left: 0;
    width: 100%;
    min-height: calc(100% - 711px);
    background-color: rgb(32, 0, 1);
    color: white;
  }

  .isling {
    margin: 0;
    padding: 0;
    background-color: rgb(253, 174, 51);
  }
</style>

<svelte:window bind:scrollY />

<div class="isling">
  <div class="parallax-container">
    {#each layers as layer}
      <img
        style="transform: translate(0,{(-scrollY * layer) / layers.length}px)"
        src="/images/parallax/parallax{layer}.png"
        alt="parallax layer {layer}" />
    {/each}
  </div>

  <div class="content">
    <div class="foreground">
      <slot {scrollY} />
    </div>
  </div>
</div>
