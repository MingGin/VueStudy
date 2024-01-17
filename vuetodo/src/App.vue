<template>
  <header class="maple-bold-title">
    <p>TODO</p>
  </header>
  <body >
    <div class="container maple-light custom-content-border" >
        <TodoInputText v-for="inpt in inpts" :key="inpt.id" :inpt = inpt @removeTodo ="removeTodo">

        </TodoInputText>
      </div>
      <div class="container maple-light custom-content" >
        <TodoAddButton @inputClick="inputAdd"/>
      </div>
  </body>
  <footer>
    <div class="maple-light custom-footer">
    </div>
  </footer>
</template>

<script setup>
import TodoAddButton from './components/TodoAddButton.vue'
import TodoInputText from './components/TodoInputText.vue'
import {ref, onBeforeMount} from 'vue'
// import dayjs from 'dayjs'

/* Add TextBox */
const inpts = ref([])
const fnInputPush = function(){
  return new Promise (function(resolve, reject){ // eslint-disable-line no-unused-vars
    for(let i = 0; i < localStorage.length; i++){
      const key = window.localStorage.key(i)
      inpts.value.push(JSON.parse(localStorage.getItem(key)))
    }
  })
}

/* id 순차 정렬 */
onBeforeMount(() => {
  fnInputPush()
  inpts.value.sort(function(a,b){return (a.id < b.id ? -1:1 )})
});

/* 할일 추가 */
let count = 0
const inputAdd = (index) =>{
  const todoItem = {
    id: index+count++,
    text: "오늘의 할일을 입력하세요.",
    checked: false,
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