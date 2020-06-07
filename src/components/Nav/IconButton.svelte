<script>
  export let name;
  export let icon;
  export let href;
  export let active;
  export let prefetch;
</script>

<style>
  a {
    height: 72px;
    width: 72px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-flow: column;
    text-decoration: none;
    color: #606060;
  }

  a:hover {
    background-color: rgba(1, 1, 1, 0.08);
    color: #f25041;
  }

  a.active {
    color: #f25041;
  }

  icon {
    font-size: 24px;
  }

  div.name {
    margin-top: -6px;
    font-size: 14px;
  }
</style>

<a {href} class:active rel={prefetch ? 'prefetch' : undefined}>
  <icon class={`mdi ${icon}`} />
  <div class="name">{name}</div>
</a>
