<template>

    <main class="app">

        <section class="main">
            <h3>My ToDoList </h3>
                <input type="text" placeholder="What do you have planned today?" v-model="input_content">

                <input class="addbtn" type="submit" value="Add" @click="addTodo">
        </section>

        <section class="todo-list">
            <ul>
                <li v-for="(todo, index) in todos" :key="index">
                    <input type="checkbox" v-model="todo.done" :true-value="true" :false-value="false"> 
                    <span :class="{'done': todo.done}" v-show="todo.done === true"> {{todo.content }} </span>
                    <span class="item" v-if="!todo.editing" v-show="todo.done !== true">{{ todo.content }}</span>
                    <input v-else v-model="todo.content" @keyup.enter="saveEdit(index)">
                    <button class="edit" @click="toggleEdit(index)">{{ todo.editing ? 'Save' : 'Edit' }}</button>
                    <button class="delete" @click="removeTodo(index)"> Delete </button>
                </li>
            </ul>

        </section>
    </main>

    
</template>

<style>

.app {
    width: 400px;
    height: 400px;
    background-color: #225A74;
    position: relative;
}

.main {
  width: 100%;
}

.main h3{
    font-size: 30px;
    text-align: center;
    color: white;
}

.main input {
  width: 320px;
  height: 35px;
  padding: 5px;
  border: solid 3px white;
  border-radius: 50px;
  font-size: 16px;
  position: relative;
  left: 20px;
}

.main .addbtn {
  cursor: pointer;
  width: 40px;
  height: 40px;
  margin: 10px;
  padding: 10px 3px;
  background-color: white;
  color: red;
  border:none;
  border-radius: 50%;
}

.main .addbtn:hover {
    background-color: #4D8AA6;
    color:white
}


ul{
    list-style: none;
    display: inline-block;
    width: 100%
}

li input{
    position: relative;
    left: -15px;
}

.item{
    position: relative;
    margin-left: -5px;
    color:white;
}


.edit{
    position: absolute;
    right: 60px;
    background-color: transparent;
    color: skyblue;
    font-weight: 500;
    border:none
}

.edit:hover{
    background-color: #4D8AA6;
    border:solid 1px white;
    color:white
}

.delete{
    position: absolute;
    right: 10px;
    background-color: transparent;
    color:red;
    font-weight: 500;
    border:none
}

.delete:hover{
    background-color: #4D8AA6;
    border:solid 1px white;
    color:white
}

.done {
  text-decoration: line-through;
}



/* debug
*{
     border: solid 3px white 
}*/

</style>

<script setup>
import { computed, ref, watch, onMounted } from "vue"

    const todos= ref([])
    const input_content = ref('')

    const addTodo = () => {
        if (input_content.value.trim() === ''){
            return
        }

        todos.value.push({
            content: input_content.value,
            done: false
        })

        input_content.value = ''
        
    }


    //刪除
    const removeTodo = todo => { 
        todos.value.splice(todo, 1)
    }

    const toggleEdit = todo => {
        todos.value[todo].editing = !todos.value[todo].editing
    }

    const saveEdit = todo => {
        todos.value[todo].editing = false
    }

    //將資料暫存，並轉為 JSON 格式的字串
    watch(todos, (newVal) => {
        localStorage.setItem('todos', JSON.stringify(newVal))
    },{deep: true})

    onMounted(() => {
         todos.value = JSON.parse(localStorage.getItem('todos'))|| []
    })

</script>
