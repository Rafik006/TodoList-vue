<script setup>
import { ref,watch,onMounted,computed } from "vue";

const todos=ref([])
const content_input=ref('')
const sortedTodos=computed(()=>todos.value.sort((a,b)=>{
  return b.createdAt-a.createdAt
}))
const addTodo=()=>{
  todos.value.push({
    content:content_input.value,
    completed:false,
    createdAt:new Date().getTime()
  })
  content_input.value=""
}

watch(todos,newValue=>{
  localStorage.setItem("todos",JSON.stringify(newValue))
  
},{deep:true})
onMounted(()=>{
  const storedTodos=JSON.parse(localStorage.getItem('todos') ) || []
  todos.value=storedTodos
  // console.log(todos)
})
const deleteTodo=(todo)=>{
  todos.value=todos.value.filter(t=> t!== todo)
}
</script>

<template id="todo-container">
  <header>
    <div class="title-container">
      <h1  class="a">Todo App </h1>
    </div>
    <div class="input-container">
      <input type="text" placeholder="my todo ... " v-model="content_input" />
      <button @click="addTodo">Add Todo</button>
    </div>
  </header>
  <section  class="todos-container">
    <div  v-for="todo in sortedTodos" class="todo-container">
      <div class="todo-content">
        <input type="radio" id="completed" >
        <span class="todo-item">{{todo.content}}</span>
      </div>
      <span class="delete-btn" @click="deleteTodo(todo)">DELETE</span>
    </div>
  </section>
</template>
<style>
header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2em;
  /* widows: ;/ */
  width: 50%;
  margin: 0 auto;
}
.title-container {
  height: 6em;
}
h1 {
  /* font-weight: bold; */
  /* color: vla; */
  font-size: 1.4em;
  letter-spacing: 0.2em;
  text-transform: capitalize;
  transition: 2s ease-in-out;
}

.visible {
  display: block;
}
.input-container {
  display: flex;
  gap: 1em;
  align-items: center;
}
input {
  width: 30em;
  padding: 1.2em;
  font-size: 1.4em;
  font-weight: bold;
  outline: none;
  border-radius: 0.8em;
  letter-spacing: 0.2em;
  text-transform: capitalize;
}
.a{
  font-size: 1.4em;
  font-weight: bold;
  letter-spacing: 0.2em;
  height: 5em;
}
button {
  width: 8em;
  height: 3em;
  font-weight: bold;
  background-color: rgba(254, 69, 69, 0.757);
  border: none;
  color: white;
  font-size: 20px;
  letter-spacing: 0.1em;
  font-weight: bold !important;
  border-radius: 0.2em;
}
.todos-container {
  margin: 5em auto;
  background-color: white;
  height: 30em;
  width: 40%;
  border-radius: 0.5em;
}
.todo-item{

  width:60%;
}
.delete-btn{
  color: red;
  background-color: white;
  padding: 0.2em;
  font-weight: 900;
  border-radius: 0.2em;
  transition: 0.2s ease-in-out;
  cursor: pointer;

}
.delete-btn:hover{
  padding: 0.4em;
}
.todo-container{
  width: 80%;
  background-color: black;
  height: 4em;
  display: flex;
  margin: 2em auto;
  align-items: center;
  justify-content: space-between;
  padding: 1em;
  border-radius: 0.3em;
}
#completed{
  width: 2em;
}
.todo-content{
  width: 70%;
  color: white;
  font-size:1.2em;
  letter-spacing: 0.1em;
  
}


</style>
