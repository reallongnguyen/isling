<script>
  import IconButton from "./IconButton.svelte";
  export let segment;

  let scrollY;
  $: bgOpacity =
    Math.min(1, scrollY / 711) < 0.5 ? 0 : Math.min(0.9, scrollY / 711);
</script>

<style>
  nav {
    position: fixed;
    width: 72px;
    height: 100vh;
    background-color: white;
    opacity: 88%;
    transition: opacity 0.5s;
  }

  nav:hover {
    opacity: 100%;
  }

  ul {
    margin: 0;
    padding: 0;
  }

  /* clearfix */
  ul::after {
    content: "";
    display: block;
    clear: both;
  }

  li {
    display: block;
    float: left;
  }
</style>

<svelte:window bind:scrollY />
<nav style="background-color: rgba(255, 255, 255, {bgOpacity})">
  <ul>
    <li>
      <IconButton
        name="Home"
        icon="mdi-home"
        href="."
        active={segment === undefined} />
    </li>

    <!-- for the blog link, we're using rel=prefetch so that Sapper prefetches
		     the blog data when we hover over the link or tap it on a touchscreen -->
    <li>
      <IconButton
        name="Blog"
        icon="mdi-lava-lamp"
        href="blog"
        active={segment === 'blog'} />
    </li>
    <li>
      <IconButton
        name="Todo"
        icon="mdi-format-list-checks"
        href="todo"
        active={segment === 'todo'} />
    </li>
  </ul>
</nav>
