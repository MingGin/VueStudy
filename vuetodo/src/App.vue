<template>
  <header class="maple-bold-title">
    <p>TODO</p>
  </header>
  <body>
    <div class="maple-light custom-content">
      <ul>
        <li v-for="inpt in inpts" :key="inpt.index">{{ inpt }}</li>
      </ul>
    </div>
  </body>
  <footer>
    <div class="maple-light custom-footer">
      footer
    </div>
  </footer>
</template>

<script setup>
//import MyInputButton from './components/MyInputButton.vue'
//import MyInput from './components/MyInput.vue'
import {ref} from 'vue'

const inpts = ref([])
</script>