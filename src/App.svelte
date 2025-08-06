<script lang="ts">
  import TasksForm from "./components/tasks-form.svelte";
  import TasksList from "./components/tasks-list.svelte";
  import type { Filter, Task } from "./types";
  let message = "Tasks app";
  let currentFilter = $state<Filter>("all");
  let tasks = $state<Task[]>([]);
  let totalDone = $derived(tasks.filter(task => task.done).length);
  let filteredTasks = $derived.by(() => {
    switch (currentFilter) {
      case "all":
        return tasks;
      case "todo":
        return tasks.filter(task => !task.done);
      case "done":
        return tasks.filter(task => task.done);
    }
    return tasks;
  })

  function addTask(task: string) {
    if (task.trim() === "") return;
    tasks.push({
      id: crypto.randomUUID(),
      title: task,
      done: false,
    })
  }

  function toggleDone(task: Task) {
    task.done = !task.done
  }

  function removeTask(id: string) {
    const index = tasks.findIndex(task => task.id === id);
    tasks.splice(index, 1);
  }

</script>

{#snippet filterButton(filter: Filter)}
<button onclick={() => currentFilter = filter} 
  class:constrant={currentFilter === filter} 
  class="secondary filterButton">
   {filter}
</button>
{/snippet}

<main class="container">
  <h1>{message}</h1>
  <TasksForm {addTask} />
  {#if tasks.length > 0}
    <p>{totalDone} / {tasks.length} tasks completed</p>
    <div class="button-container">
      {@render filterButton("all")}
      {@render filterButton("todo")}
      {@render filterButton("done")}
    </div>
  {:else if tasks.length === 0}
    <p>Please add a task, your tasks are empty</p>
  {/if}
  <TasksList tasks={filteredTasks} {toggleDone} {removeTask} />
</main>


<style>
  main {
    margin: 1rem auto;
    max-width: 800px;
    padding: 1rem;
  }

  .button-container {
    display: flex;
    justify-content: end;
    margin-bottom: 1rem;
    gap: 1rem;
  }

  .filterButton {
    text-transform: capitalize;
  }
</style>