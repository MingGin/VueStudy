<template>
  <header class="maple-bold-title">
    <p>TODO</p>
  </header>
  <body>
    <div class="maple-light custom-content">
        <MyInputText :isInput = isInput v-for="inpt in inpts" :key="inpt.id" :inpt = inpt :is="inputComponent" @doubleClick="dbClick">

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
// const index = 0
const inputAdd = () =>{
    inpts.value.push({
      id: Math.floor(Math.random() * 100000000),
      text: "오늘의 할일을 입력하세요.",
      date: new Date().getDate,
      state: "Text",
    })
}
/* TextBox -> Input 변경 */
const inputComponent = ref(MyInputText)
const isInput = ref(false)
const dbClick = () =>{
  isInput.value = !isInput.value
}

</script>