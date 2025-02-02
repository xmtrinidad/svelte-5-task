<script lang="ts">
	import TasksForm from '$lib/components/tasks-form.svelte';
	import TasksList from '$lib/components/tasks-list.svelte';
	import type { Task } from '$lib/types';

	let message = 'Tasks App';
	let tasks = $state<Task[]>([])
	let totalDone = $derived(
		tasks.reduce((total, task) => total + Number(task.done), 0)
	);

	function addTask(newTask: string) {
		tasks.push({
			id: crypto.randomUUID(),
			title: newTask,
			done: false
		});
	}

	function toggleDone(task: Task) {
		task.done = !task.done;
	}
</script>

<div class="home">
	<h1>{message}</h1>
	<TasksForm {addTask} />
	<p>{totalDone} / {tasks.length} tasks completed</p>
	<TasksList {tasks} {toggleDone} />
</div>

<style>
</style>
