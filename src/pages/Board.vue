<script setup>

import { ref, onMounted, computed, watch } from 'vue'

const todo_notes = ref([])
const input_title = ref('')
const input_desc = ref('')
const input_colour = ref(null)

const todo_notes_asc = computed(() => todo_notes.value.sort((a,b) =>{ return a.createdAt - b.createdAt }))


watch(todo_notes, (newValue) => {localStorage.setItem('todo_notes', JSON.stringify(newValue))}, {deep: true})

const addTodo = () => { if (input_title.value.trim() === '' || input_desc.value.trim() === '' || input_colour.value === null) {return}

todo_notes.value.push({ titleContent: input_title.value, descContent: input_desc.value, colour: input_colour.value, done: false, editable: false, createdAt: new Date().getTime() })}

const deleteTodo = (todo) => { todo_notes.value = todo_notes.value.filter((t) => t !== todo )}

onMounted(() => { todo_notes.value = JSON.parse(localStorage.getItem('todo_notes')) || [] })

function toggleToDoCreate() {
const createTodo = document.querySelector(".create-todo");
  if (createTodo.style.display === "none") {
    createTodo.style.display = "block";
  } else {
    createTodo.style.display = "none";
  }
}

</script>


<template>


	<main class="board">
		<div class= "clickable-board" @click="toggleToDoCreate()"></div>
		<section class="create-todo" >
			<h2>Create a todo</h2>
			<form id="new-todo-form" @submit.prevent="addTodo">
				<h3>Title:</h3>
				<input type="text" name="content" id="content" placeholder="" v-model="input_title" />

				<h3>Description:</h3>
				<textarea type="text" name="content" id="content" placeholder="" v-model="input_desc"></textarea>
				
				<div class="options">
				<div>
					<label class="colour1">
						<input type="radio" name="colour" d="colour1" value="colour1" v-model="input_colour" />
					</label>
				</div>
					<label class="colour2">
						<input type="radio" name="colour" id="colour2" value="colour2" v-model="input_colour" />
					</label>
	
					<label class="colour3">
						<input type="radio" name="colour" id="colour3" value="colour3" v-model="input_colour" />
					</label>

					<label class="colour4">
						<input type="radio" name="colour" id="colour4" value="colour4" v-model="input_colour" />
					</label>

				</div>

				<input type="submit" value="Create" />
			</form>
		</section>

		<section class="todo-list">
			<div class="list" id="todo-list">

				<div v-for="todo in todo_notes_asc" :class="`todo-item ${todo.done && 'done'}`">
					<div class="todo-content">
						<input class ='title-content' type="text" v-model="todo.titleContent" />
            			<textarea class ='desc-content' type="text" v-model="todo.descContent"></textarea>
						<div class='username-content'>Created By: {{ username }}</div>
					</div>

					<div class="actions">
						<button class="delete" @click="deleteTodo(todo)">&#x2713;</button>
					</div>
				</div>

			</div>
		</section>

	</main>
</template>
