<script>
  import Nav from "../components/Nav/Nav.svelte";

  export let segment;
</script>

<style>
  header {
    top: 0;
    height: 0;
    position: relative;
    z-index: 1000;
  }

  main {
    position: relative;
    box-sizing: border-box;
    font-family: -apple-system, BlinkMacSystemFont, sans-serif;
    color: rgba(1, 1, 1, 0.8);
  }

  :global(div) {
    box-sizing: border-box;
  }
</style>

<header>
  <Nav {segment} />
</header>

<main>
  <slot />
</main>
