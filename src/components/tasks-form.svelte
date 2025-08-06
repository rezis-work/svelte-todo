<script lang="ts">
  let newTask: string = $state<string>("")
    let {addTask}: {addTask: (task: string) => void} = $props();

  function formSubmitted(event: Event) {
    event.preventDefault();
    addTask(newTask);
    newTask = "";
  }
</script>

<form onsubmit={formSubmitted}>
  <label>
    <input type="text" name="newTask" bind:value={newTask} placeholder="Add a new task" />
  </label>
  <div class="button-container">
    <button>Add Task</button>
  </div>
</form>

<style>
  form {
    margin-bottom: 1rem;
  }
  .button-container {
    display: flex;
    justify-content: flex-end;
  }
</style>