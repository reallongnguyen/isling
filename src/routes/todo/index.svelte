<script>
  import { onMount } from "svelte";
  import Todo from "./Todo.svelte";

  let i = 0;
  const autoInscrement = () => i++;

  let todos = [
    {
      id: autoInscrement(),
      name: "Create a todo tool",
      status: "INPROGRESS",
      startedAt: new Date()
    },
    {
      id: autoInscrement(),
      name: "Learn Go programing language",
      status: "INPROGRESS",
      startedAt: new Date(),
      runTime: 1200
    },
    {
      id: autoInscrement(),
      name: "Create one project by svelte",
      status: "WAITING"
    },
    { id: autoInscrement(), name: "Take a trip", status: "DONE" }
  ];

  const startTask = id => {
    const todo = todos.find(t => t.id === id);
    todo.status = "INPROGRESS";
    todo.startedAt = new Date();

    todos = [...todos];
  };

  const pauseTask = id => {
    const todo = todos.find(t => t.id === id);
    todo.status = "WAITING";
    todo.startedAt = null;

    todos = [...todos];
  };

  const formatTimeEslap = second => {
    if (isNaN(second)) {
      return "";
    }

    let min = Math.floor(second / 60);
    second = second % 60;

    let hour = Math.floor(min / 60);
    min = min % 60;

    return hour === 0
      ? `
      ${min > 0 ? `${min}m` : ""}
      ${second < 10 ? `0${second}` : second}s
    `
      : `
      ${hour > 0 ? `${hour}h` : ""}
      ${min > 0 ? `${min}m` : ""}
    `;
  };

  onMount(() => {
    const countRunTime = () => {
      todos
        .filter(t => t.status === "INPROGRESS")
        .forEach(t => (t.runTime = isNaN(t.runTime) ? 1 : t.runTime + 1));

      todos = [...todos];
    };

    const id = setInterval(countRunTime, 1000);

    return () => clearInterval(id);
  });
</script>

<style>
  .container {
    width: cacl(100% - 72px);
    height: 100vh;
    overflow: hidden;
    padding-top: 40px;
    margin-left: 72px;
    background-color: #f5f5f5;
  }

  .todo {
    width: 25vw;
  }
</style>

<div class="container">
  <div class="todo">
    <Todo {todos} {startTask} {pauseTask} />
  </div>
</div>
