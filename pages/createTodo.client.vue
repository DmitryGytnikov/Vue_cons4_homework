<script setup>
import { InfoFilled } from "@element-plus/icons-vue"

const router = useRouter()

const tasks = ref([])

const task = ref("")

const subtasks = ref([
	{
		subId: 0,
		subDescription: "",
	},
])

const getTodos = () => {
	tasks.value = JSON.parse(localStorage.getItem("tasks"))
}

getTodos()

const saveTask = () => {
	if (task.value.length > 0) {
		subtasks.value = subtasks.value.filter(
			subtask => subtask.subDescription !== ""
		)

		for (let i = 0; i < subtasks.value.length; i++) {
			subtasks.value[i].subId = i
		}

		tasks.value.push({
			id: Math.random() * 100000000000000000,
			description: task.value,
			isCompleted: false,
			subtasks: subtasks.value,
		})

		localStorage.setItem("tasks", JSON.stringify(tasks.value))

		router.push({ name: "index" })
	}
}

const cancelSaveTask = () => {
	task.value = ""
	subtasks.value = [
		{
			subId: 0,
			subDescription: "",
		},
	]

	router.push({ name: "index" })
}

const deleteSubtask = idx => {
	if (subtasks.value.length > 1) {
		subtasks.value = subtasks.value.filter(subtask => subtask.subId !== idx)

		for (let i = 0; i < subtasks.value.length; i++) {
			subtasks.value[i].subId = i
		}
	}
}

const addSubtask = () => {
	subtasks.value.push({
		subId: subtasks.value.length,
		subDescription: "",
	})
}
</script>

<template>
	<div class="container--create">
		<h2>Создать задачу</h2>
		<label class="create__label"
			>Название задачи1
			<el-input
				v-model="task"
				class="create__input"
				placeholder="Введите задачу"
			/>
		</label>
		<div
			class="create__add-subtask--wr"
			v-for="(subtask1, subIndex) in subtasks"
			:key="subtask1.subId"
		>
			<label class="create__label">
				<!-- <div>{{ subtasks }}</div> -->
				Описание подзадачи № {{ subIndex + 1 }}
				<el-input
					v-model="subtask1.subDescription"
					class="create__input"
					placeholder="Введите подзадачу"
				/>
			</label>
			<el-button
				@click="deleteSubtask(subtask1.subId)"
				class="create__delete-subtask"
				type="danger"
				plain
			>
				Удалить
			</el-button>
		</div>

		<el-button
			@click="addSubtask"
			class="create__add-subtask"
			type="primary"
			plain
		>
			Добавить подзадачу
		</el-button>

		<el-button-group class="create__container">
			<el-popconfirm
				width="300"
				:icon="InfoFilled"
				icon-color="#ff9900"
				title="Вы уверены, что хотите отменить создание задачи?"
				@confirm="cancelSaveTask"
			>
				<template #reference>
					<el-button class="create__cancel" type="danger" plain>
						Отменить
					</el-button>
				</template>
				<template #actions="{ confirm, cancel }">
					<el-button
						@click="cancel"
						plain
						style="width: 50px; border: 2px solid #409eff; border-radius: 5px"
						type="primary"
					>
						Нет
					</el-button>
					<el-button
						@click="confirm"
						plain
						style="width: 50px; border: 2px solid #409eff; border-radius: 5px"
						type="primary"
					>
						Да
					</el-button>
				</template>
			</el-popconfirm>

			<el-button @click="saveTask" class="create__save" type="primary" plain>
				Сохранить
			</el-button>
		</el-button-group>
	</div>
</template>

<style scoped>
*,
*::before,
*::after {
	padding: 0;
	margin: 0;
	border: 0;
	box-sizing: border-box;
}

.container--create {
	width: 412px;
	margin: 0 auto;
	padding: 12px;
	background-color: #e6edf9;
	border-bottom: 2px solid #ffffff;
}

.container--create h2 {
	margin-bottom: 12px;
	font-weight: 400;
	font-style: normal;
	font-size: 24px;

	text-align: center;
}

.create__label {
	display: block;
}

.create__input {
	margin-bottom: 18px;
	padding-top: 8px;
}

.create__container {
	display: flex;
}

.create__cancel {
	border: 2px solid #facbcb;
	border-radius: 5px;
	width: 50%;
}

.create__cancel:hover,
.create__cancel:focus,
.create__cancel:active {
	border: 2px solid #f56c6c;
	background-color: #f56c6c;
	color: #fffff6;
}

.create__save {
	border: 2px solid #a0cfff;
	border-radius: 5px;
	width: 50%;
	margin-left: 0;
}

.create__save:hover,
.create__save:focus,
.create__save:active {
	border: 2px solid #409eff;
	background-color: #409eff;
	color: #f3ffff;
}

.create__add-subtask--wr {
	margin-bottom: 18px;
}

.create__delete-subtask {
	width: 84px;
	border: 2px solid #facbcb;
	border-radius: 5px;
}

.create__delete-subtask:hover,
.create__delete-subtask:focus,
.create__delete-subtask:active {
	border: 2px solid #f56c6c;
	background-color: #f56c6c;
	color: #fffff6;
}

.create__add-subtask {
	width: 100%;
	border: 2px solid #a0cfff;
	border-radius: 5px;
	margin-left: 0;
	margin-bottom: 18px;
}

.create__add-subtask:hover,
.create__add-subtask:focus,
.create__add-subtask:active {
	border: 2px solid #409eff;
	background-color: #409eff;
	color: #f3ffff;
}
</style>
