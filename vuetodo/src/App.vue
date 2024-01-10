<template>
  <header class="maple-bold-title">
    <p>TODO</p>
  </header>
  <body>
    <div class="maple-light custom-content">
        <MyInputText v-for="inpt in inpts" :key="inpt.id" :inpt = inpt @removeTodo ="removeTodo">

        </MyInputText>
      <MyInputButton @inputClick="inputAdd"/>
    </div>
  </body>
  <footer>
    <div class="maple-light custom-footer">
    </div>
  </footer>
</template>

<script setup>
import MyInputButton from './components/MyInputButton.vue'
// import MyInput from './components/MyInput.vue'
import MyInputText from './components/MyInputText.vue'
import {ref} from 'vue'

/* Add TextBox */
const inpts = ref([])
for(let i = 0; i < localStorage.length; i++){
  const key = window.localStorage.key(i)
  inpts.value.push(JSON.parse(localStorage.getItem(key)))
}

// let index = 0
const inputAdd = () =>{
  const todoItem = {
    id: Math.floor(Math.random() * 100000000),
    text: "오늘의 할일을 입력하세요.",
    date: new Date().getDate
  }
  localStorage.setItem(todoItem.id,JSON.stringify(todoItem))
  inpts.value.push(todoItem)
}
/* id로 todo 찾기 */
const targetTodoFilter = function(todos,todoId){
  for(let i=0; todos.length>i; i++){
    if(todos[i].id == todoId){
      return i
    }
  }
}
/* TODO 삭제 */
const removeTodo = (itemId)=>{
  const targetTodo = targetTodoFilter(inpts.value,itemId)
  inpts.value.splice(targetTodo,1);
  localStorage.removeItem(itemId)
}

</script>