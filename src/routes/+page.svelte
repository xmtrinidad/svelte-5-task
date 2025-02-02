<script lang="ts">
	import TasksForm from '$lib/components/tasks-form.svelte';
	import TasksList from '$lib/components/tasks-list.svelte';
	import { type Filter, type Task } from '$lib/types';

	let message = 'Tasks App';
	let currentFilter = $state<Filter>('all');
	let tasks = $state<Task[]>([])
	let totalDone = $derived(
		tasks.reduce((total, task) => total + Number(task.done), 0)
	);

	let filteredTasks = $derived.by(() => {
		switch (currentFilter) {
			case 'all':
				return tasks;
			case 'todo':
				return tasks.filter(task => !task.done);
			case 'done':
				return tasks.filter(task => task.done);
		}
	});

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

	function removeTask(id: string) {
		const index = tasks.findIndex(task => task.id === id);
		tasks.splice(index, 1);
	}
</script>

{#snippet filterButton(filter: Filter)}
<button class:contrast={currentFilter === filter} onclick={() => currentFilter = filter} class="secondary filterButton">{filter}</button>
{/snippet}

<div class="home">
	<h1>{message}</h1>
	<TasksForm {addTask} />
	{#if tasks.length}
		<p>{totalDone} / {tasks.length} tasks completed</p>
		<div class="button-container">
			{@render filterButton('all')}
			{@render filterButton('todo')}
			{@render filterButton('done')}
		</div>
	{/if}
	
	<TasksList tasks={filteredTasks} {toggleDone} {removeTask} />
</div>

<style>
	.button-container {
		display: flex;
		justify-content: flex-end;
		gap: 0.5rem;
	}

	.filterButton {
		text-transform: capitalize;
	}
</style>
