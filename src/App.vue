<template>
	<div id="app">
		<div class="container-fluid">
			<div class="">
				<div class="container col-12 cel">
					<h1 class="title">TODO</h1>
				</div>
				<div class="container col-12 cel">
					<TaskProgress :progress="progress" />
				</div>
				<div class="container col-12 cel">
					<NewTask @taskAdded="addTask" />
				</div>
				<div class="container col-12 cel">
					<taskGrid @taskDeleted="deleteTask" @taskStateChanged="changeTask" :tasks="tasks" />
				</div>
			</div>
		</div>

	</div>
</template>

<script>
import TaskGrid from './components/TaskGrid.vue'
import NewTask from './components/NewTask.vue'
import TaskProgress from './components/TaskProgress.vue'
export default {
	components: { TaskGrid, NewTask, TaskProgress },
	data() {
		return {
			tasks: []
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0
		}
	},
	watch: {
		tasks: {
			deep: true,
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			if (reallyNew) {
				this.tasks.push({
					name: task.name,
					pending: task.pending || true
				})
			}
		},
		deleteTask(i) {
			this.tasks.splice(i, 1)
		},
		changeTask(i) {
			this.tasks[i].pending = !this.tasks[i].pending
		}
	},
	created() {
		const json = localStorage.getItem('tasks')
		const array = JSON.parse(json)
		console.log(array)
		this.tasks = Array.isArray(array) ? array : []
	},
}
</script>

<style>
body {
	font-family: 'Lato', sans-serif;
	background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
	color: #FFF;
}


#app h1 {
	margin-bottom: 5px;
	font-weight: 300;
	font-size: 3rem;
}

.cel {
	margin-bottom: 12px;
}

.title {
	text-align: center;
	margin-top: 12px;
}
</style>
