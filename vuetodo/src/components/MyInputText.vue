<template>
    <div class="button_box div_solid" @focusout="fnInputDetail()">
        <input type="checkbox" class="form-check-input input_box" v-model="checked"/>
        <input v-model="inputText2" id="inptBox" v-if = "isInputRef" class="form-control maple-light input"  placeholder="오늘의 할일을 입력하세요." v-focus @keyup.enter="fnInputDetail()" /> 
        <text :class="classObject" v-else class="maple-light input" @dblclick="fnTextDbClick()">{{ inputText }}</text>
        <button class="btn btn-danger" v-show="!isInputRef" @click="$emit('removeTodo', itemId)">delete</button>
    </div>
</template>

<script setup>
// import MyInput from './MyInput.vue'
import {ref, reactive} from 'vue';
import {defineProps} from 'vue';

    const props = defineProps({                  
        inpt: {
            id: {
                type: Number,
                required: true
            },
            text: {
                type: String,
                required : true
            },
            date: {
                type: Date,
                required : true
            }
        }
    })
    /* text -> input 변경 */
    const isInputRef = ref(false)

    const fnTextDbClick = function() {
        isInputRef.value = !isInputRef.value;
        fnInputText()        
    }
    
    const fnInputDetail = function() {   
        if(isInputRef.value == true){
            isInputRef.value = !isInputRef.value;
            fnInputText()
        }
    }
    /* onFocus */
    const vFocus = {
        mounted: (el) => el.focus()
    }

    /* text */
    let itemId = ref(props.inpt.id)
    let inputText = JSON.parse(localStorage.getItem(props.inpt.id)).text
    const inputText2 = ref("")

    const fnInputText = function() {   
        if(inputText2.value.length > 0){
            inputText = inputText2.value
            const todoItem = {
                id: props.inpt.id,
                text: inputText2.value,
                date: new Date().getDate
            }
            localStorage.setItem(todoItem.id,JSON.stringify(todoItem))
        } else{
            inputText = JSON.parse(localStorage.getItem(props.inpt.id)).text
        }
    }
    /* checkBox */
    const checked = ref()
    const classObject = reactive({
        text_deco: checked
    })

</script>
