<template>
    <div>
      <v-progress-linear
        v-model="compleCnt"
        height="57"
        color="primary"
      >
        <strong>{{ Math.ceil(compleCnt) }}%</strong>
      </v-progress-linear>
    </div>
  </template>

<script setup>
import { ref, defineProps, watch } from 'vue'

const props = defineProps({                  
    count: {
        type: Number,
        default: 0  
    }
})

const compleCnt =ref(0)
watch (
  () => props.count,
  (count) => {
    compleCnt.value = count
    console.log(compleCnt.value)
    }
)


</script>