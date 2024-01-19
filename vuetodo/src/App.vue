<template>
  <header class="maple-bold-title">
    <p>TODO</p>
    <TodoCalendar :count = "TodoCompleteCnt" @todoDate = "todoDate"></TodoCalendar>
  </header>
  <body >
    <div class="container maple-light" :class = "{'custom-content-border': isBorder}" >
        <TodoInputText v-for="inpt in inpts" :key="inpt.id" :inpt = inpt @removeTodo ="removeTodo" @checkComplete="checkComplete">

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
import {ref, onBeforeMount, watch} from 'vue'

const inpts = ref([])
const date = ref()
const isBorder = ref(true)
const TodoCompleteCnt = ref(0)
let compleCnt = 0

/* Todo ChekBox 클릭시 */
const checkComplete = (inpt,checked)=>{
  inpt.value.checked = checked
  if(checked){
    compleCnt++
  } else {
    compleCnt--
  }
  TodoCompleteCnt.value = compleCnt/inpts.value.length * 100
  return TodoCompleteCnt
}

/* Todo 성공 갯수 */
watch(TodoCompleteCnt,(cnt)=>{
  TodoCompleteCnt.value = cnt
  console.log(TodoCompleteCnt.value)
})

/* Lendering Add TextBox */
const fnInputPush = function(){
  return new Promise (function(resolve, reject){ // eslint-disable-line no-unused-vars
    compleCnt = 0
    for(let i = 0; i < localStorage.length; i++){
      const key = window.localStorage.key(i)
      if(JSON.parse(localStorage.getItem(key)).id.substring(0,8) == date.value){
        inpts.value.push(JSON.parse(localStorage.getItem(key)))
      }
    }
    compleCnt = fnChecked()
    TodoCompleteCnt.value = compleCnt/inpts.value.length * 100
  })
}
/* checked true 찾기 */
const fnChecked = ()=>{
  let cnt = 0
  for(let i = 0; i < inpts.value.length; i++){
    if(inpts.value[i].checked){
      cnt++
    }
  }
  return cnt
}

watch(inpts, (newInpts) => {
    if(newInpts.length <= 0){
      isBorder.value = false
    } else{
      isBorder.value = true
    }
})


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
  compleCnt = fnChecked()
  TodoCompleteCnt.value = compleCnt/inpts.value.length * 100
  if(inpts.value.length <= 0){
    isBorder.value = false
  } else{
    isBorder.value = true
  }
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
  compleCnt = fnChecked()
  console.log("delete : " + compleCnt)
  TodoCompleteCnt.value = compleCnt/inpts.value.length * 100
  if(inpts.value.length <= 0){
    isBorder.value = false
  } else{
    isBorder.value = true
  }
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