<template>
  <header class="maple-bold-title">
    <p>TODO</p>
    <TodoCalendar @todoDate = "todoDate"></TodoCalendar>
  </header>
  <body >
    <div class="container maple-light custom-content-border" >
        <TodoInputText v-for="inpt in inpts" :key="inpt.id" :inpt = inpt @removeTodo ="removeTodo">

        </TodoInputText>
      </div>
      <div class="container maple-light custom-content" >
        <TodoAddButton :propDate="date" @inputClick="inputAdd"/>
      </div>
  </body>
</template>

<script setup>
import TodoAddButton from './components/TodoAddButton.vue'
import TodoInputText from './components/TodoInputText.vue'
import TodoCalendar from './components/TodoCalendar.vue'
import {ref, onBeforeMount} from 'vue'

const date = ref()
const isAlert = ref(false)

/* Add TextBox */
const inpts = ref([])
const fnInputPush = function(){
  return new Promise (function(resolve, reject){ // eslint-disable-line no-unused-vars
    for(let i = 0; i < localStorage.length; i++){
      const key = window.localStorage.key(i)
      if(JSON.parse(localStorage.getItem(key)).id.substring(0,8) == date.value){
        inpts.value.push(JSON.parse(localStorage.getItem(key)))
      }
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

/* 캘린더에서 날짜값 가져오기 */
const  todoDate = (todoDate) => {
  inpts.value = []
  const year = new Date(todoDate).getFullYear()
  const month = new Date(todoDate).getMonth()+1
  const day = new Date(todoDate).getDate()
  date.value = year + '' + dateFrommat(month) + '' + dateFrommat(day)
  fnInputPush()
  inpts.value.sort(function(a,b){return (a.id < b.id ? -1:1 )})

}

/* 월, 일자가 10 이하이면 0앞에다 붙여주기 */
const dateFrommat = (date) =>{
  if(date < 10){
    return '0' + date
  }
  return date
}
</script>