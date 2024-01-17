<template>
    <div class="button_box">
        <v-btn stacked variant="outlined" @click="$emit('inputClick',index())">할일 추가</v-btn>
    </div>
</template>

<script setup>
import dayjs from 'dayjs'
// import { watch } from 'vue'

let index = ()=>{ return dayjs().format('YYYYMMDDHHmmss')}

</script>