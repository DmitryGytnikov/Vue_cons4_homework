<script setup>
const router = useRouter()

const tasks = ref([
	{
		id: 0,
		description: "Изучить основы Vue",
		isCompleted: true,
	},
])

const fallbackTodos = [
	{
		id: 0,
		description: "Изучить основы Vue",
		isCompleted: true,
	},
	{
		id: 1,
		description: "Подготовиться к собеседованию",
		isCompleted: false,
	},
	{
		id: 2,
		description: "Устроиться на работу",
		isCompleted: false,
	},
]

const getTodos = () => {
	if (!localStorage.getItem("tasks")) {
		localStorage.setItem("tasks", JSON.stringify(fallbackTodos))
	}

	tasks.value = JSON.parse(localStorage.getItem("tasks"))
}

getTodos()

const setCompletion = id => {
	tasks.value.find(task => task.id === id).isCompleted = !tasks.value.find(
		task => task.id === id
	).isCompleted

	localStorage.setItem("tasks", JSON.stringify(tasks.value))
}

const deleteTask = id => {
	tasks.value = tasks.value.filter(task => task.id !== id)

	localStorage.setItem("tasks", JSON.stringify(tasks.value))
}

const editTask = idTask => {
	router.push({ name: "editTodo-id", params: { id: idTask } })
}
</script>

<template>
	<!-- <ClientOnly> -->
	<div class="container--home">
		<h2>Список задач</h2>
		<NuxtLink to="/createTodo" class="create-link"> Создать задачу </NuxtLink>
		<div
			v-for="(task, index) in tasks"
			:key="task.id"
			class="task"
			:class="{ 'task--completed': task.isCompleted }"
			@click="setCompletion(task.id)"
		>
			<p class="task__number">{{ index + 1 }} .</p>
			<p class="task__text">{{ task.description }}</p>
			<div>
				<button @click.stop="deleteTask(task.id)" class="task__delete">
					Удалить
				</button>
				<button @click.stop="editTask(task.id)" class="task__edit">
					Редактировать
				</button>
			</div>
		</div>
	</div>
	<!-- </ClientOnly> -->
</template>

<style scoped>
body {
	margin: 0;
}

*,
*::before,
*::after {
	padding: 0;
	margin: 0;
	border: 0;
	box-sizing: border-box;
}

.container--home {
	width: 412px;
	margin: 0 auto;
	padding: 12px;
	background-color: #e6edf9;
	border-bottom: 2px solid #ffffff;
}

.container--home h2 {
	margin-bottom: 12px;
	font-weight: 400;
	font-style: normal;
	font-size: 24px;

	text-align: center;
}

.create-link {
	display: block;

	padding: 8px 15px;
	margin-bottom: 8px;

	text-align: center;
	font-size: 16px;
	font-weight: 500;

	line-height: 1;
	color: #67c23a;

	outline: none;
	text-decoration: none;
	border: 2px solid #b3e19d;
	border-radius: 5px;

	cursor: pointer;
	text-decoration-line: none;

	transition: all 0.3s ease;
}

.create-link:hover,
.create-link:focus,
.create-link:active {
	background-color: #67c23a;
	color: #fffff3;
	border: 2px solid #67c23a;
}

.task {
	border: 1px solid #9ca3af;
	border-radius: 5px;
	min-height: 96px;
	width: 100%;

	margin-bottom: 20px;
	padding: 12px;

	display: flex;
	align-items: center;
	cursor: pointer;
}

.task--completed {
	background-color: rgb(220 252 231);
}

.task--completed .task__text {
	text-decoration: line-through;
}

.task__number {
	width: 30px;
	flex-shrink: 0;
}

.task__text {
	flex-grow: 1;
	padding-right: 10px;
}

.task div {
	flex-shrink: 0;
	align-self: stretch;
	width: 130px;

	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

.task__delete {
	height: 32px;
	width: 100%;

	font-weight: 600;
	color: #f56c93;

	border: 2px solid #facbcb;
	border-radius: 5px;
	background-color: #fef0f0;

	display: flex;
	justify-content: center;
	align-items: center;

	cursor: pointer;

	transition: all 0.3s ease;
}

.task__delete:hover,
.task__delete:focus,
.task__delete:active {
	background-color: #f56c6c;
	color: #fffff6;
	border: 2px solid #f56c6c;
}

.task__edit {
	height: 32px;
	width: 100%;

	font-weight: 600;
	color: #40b4ff;

	border: 2px solid #a0cfff;
	border-radius: 5px;
	background-color: #ecf5ff;

	display: flex;
	justify-content: center;
	align-items: center;

	cursor: pointer;
	outline: none;
	text-decoration-line: none;

	transition: all 0.3s ease;
}

.task__edit:hover,
.task__edit:focus,
.task__edit:active {
	background-color: #409eff;
	color: #f3ffff;
	border: 2px solid #409eff;
}
</style>
