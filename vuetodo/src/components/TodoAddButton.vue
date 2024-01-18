<template>
    <div class="button_box">
        <v-btn stacked variant="outlined" @click="$emit('inputClick',index())">할일 추가</v-btn>
    </div>
</template>

<script setup>
import dayjs from 'dayjs'
import { defineProps } from 'vue'

const props = defineProps({                  
    propDate: {
            type: String,
            required : false
    }
})

/* 값을 받아오는 날짜 millisecond 까지 넘겨주기 */
let index = ()=>{ 
    return props.propDate + dayjs().format('hhmmsss')
}

</script>