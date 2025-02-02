<script lang="ts">
	import type { Task } from "$lib/types";

  let { tasks, toggleDone }: {
    tasks: Task[];
    toggleDone: (task: Task) => void;
  } = $props();
</script>

<section>
  {#each tasks as task}
    <article>
      <label for="task-{task.id}">
        <input onchange={() => toggleDone(task)} type="checkbox" id="task-{task.id}" checked={task.done} />
        <span class:done={task.done}>{task.title}</span>
      </label>
    </article>
  {/each}
</section>

<style>
  .done {
    text-decoration: line-through;
  }
</style>