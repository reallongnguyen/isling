<script>
  export let todos;
  export let startTask;
  export let pauseTask;

  $: todoList = todos.filter(t => t.status !== "DONE");
  $: doneList = todos.filter(t => t.status === "DONE");
  $: doneCount = doneList.length;
  $: inProgressCount = todos.filter(t => t.status === "INPROGRESS").length;
  $: waitingCount = todos.filter(t => t.status === "WAITING").length;
  $: donePercent = Math.round((doneCount / todos.length) * 100);

  const formatTimeEslap = second => {
    if (isNaN(second)) {
      return "";
    }

    let min = Math.floor(second / 60);
    second = second % 60;

    let hour = Math.floor(min / 60);
    min = min % 60;

    return `
      ${hour > 0 ? `${hour}h` : ""}
      ${min > 0 ? `${min}m` : ""}
      ${second < 10 ? `0${second}` : second}s
    `;
  };
</script>

<style>
  .container {
    padding: 0 24px;
  }

  .board + .board {
    margin-top: 64px;
  }

  .todo {
    height: 48px;
    font-size: 1.2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  label {
    position: relative;
    user-select: none;
    line-height: 1.2;
    display: flex;
    align-items: center;
    justify-content: start;
  }

  label.inprogress {
    color: #f25d27;
  }

  label .checkbox {
    margin-right: 8px;
    color: rgba(1, 1, 1, 0.3);
  }

  label .done {
    margin-right: 8px;
    color: #03a678;
  }

  .todo button {
    cursor: pointer;
    border: none;
    width: 32px;
    height: 32px;
    text-indent: -9999;
    border-radius: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    outline: none;
    transition: all 0.3s ease-out;
    box-sizing: border-box;
    margin-left: 4px;
  }

  .todo button:active {
    filter: brightness(1.2);
  }

  .todo button icon {
    font-size: 20px;
  }

  .todo button:hover icon {
    color: whitesmoke;
  }

  .todo button.error:hover {
    background-color: #f25041;
  }

  .todo button.error:active {
    filter: brightness(2);
  }

  .todo button.warning:hover {
    background-color: #f2955e;
  }

  .todo button.warning:active {
    filter: brightness(1.4);
  }

  .todo button.info:hover {
    background-color: #91d7f2;
  }

  .todo button.info:active {
    filter: brightness(1.2);
  }

  .menu-bar {
    display: flex;
    align-items: center;
  }

  .timer {
    color: #99aabf;
    padding-left: 4px;
    font-family: "Courier New", Courier, monospace;
  }

  .todo-statistic {
    padding-top: 40px;
  }

  .todo-statistic p {
    line-height: 1.8;
    padding: 0;
    margin: 0;
    color: rgba(1, 1, 1, 0.6);
  }

  .todo-statistic .count + .count::before {
    content: "·";
    margin: 0 8px;
  }

  .green {
    color: #03a678;
  }

  .orange {
    color: #f2955e;
  }

  .blue {
    color: dodgerblue;
  }
</style>

<svelte:head>
  <title>Todo</title>
</svelte:head>

<div class="container">
  <div class="board">
    <h2>Todo</h2>
    {#each todoList as todo}
      <div class="todo">
        <label class:inprogress={todo.status === 'INPROGRESS'}>
          <icon class="mdi mdi-checkbox-blank-outline checkbox" />
          {todo.name}
        </label>
        <div class="menu-bar">
          {#if !isNaN(todo.runTime)}
            <div class="timer">{formatTimeEslap(todo.runTime)}</div>
          {/if}
          {#if todo.status === 'WAITING'}
            <button class="info" on:click={() => startTask(todo.id)}>
              <icon class="mdi mdi-play" />
            </button>
          {/if}
          {#if todo.status === 'INPROGRESS'}
            <button class="warning" on:click={() => pauseTask(todo.id)}>
              <icon class="mdi mdi-pause" />
            </button>
          {/if}
          {#if todo.status === 'DONE'}
            <button class="error">
              <icon class="mdi mdi-close" />
            </button>
          {/if}
        </div>
      </div>
    {/each}
  </div>
  <div class="board">
    <h2>Done</h2>
    {#each doneList as todo}
      <div class="todo">
        <label class:inprogress={todo.status === 'INPROGRESS'}>
          <icon class="mdi mdi-checkbox-marked-outline done" />
          {todo.name}
        </label>
        <div class="menu-bar">
          {#if todo.status === 'DONE'}
            <button class="error">
              <icon class="mdi mdi-close" />
            </button>
          {/if}
        </div>
      </div>
    {/each}
  </div>

  <div class="todo-statistic">
    <p>{donePercent}% off all tasks complete</p>
    <p>
      <span class="count">
        <span class="green">{doneCount}</span>
        done
      </span>
      <span class="count">
        <span class="orange">{inProgressCount}</span>
        in-progress
      </span>
      <span class="count">
        <span class="blue">{waitingCount}</span>
        waiting
      </span>
    </p>
  </div>
</div>
