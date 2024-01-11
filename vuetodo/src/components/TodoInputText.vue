<template>
    <div class="button_box div_solid" @focusout="fnInputDetail()">
        <input type="checkbox" class="form-check-input input_box" v-model="checked" @click="checkedEvent"/>
        <input v-model="inputText2" id="inptBox" v-if = "isInputRef" class="form-control maple-light input"  placeholder="오늘의 할일을 입력하세요." v-focus @keyup.enter="fnInputDetail()" /> 
        <text :class="classObject" v-else class="maple-light input" @dblclick="fnTextDbClick()">{{ inputText }}</text>
        <div class="button_box div_solid">
            <button class="btn btn-danger" v-show="!isInputRef" @click="$emit('removeTodo', itemId)">delete</button>
        </div>
    </div>
</template>

<script setup>
import {ref, reactive, watch} from 'vue';
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
        checked: {
            type: Boolean,
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

/* input에 입력된 값의 따라 text 저장*/
let itemId = ref(props.inpt.id)
let inputText = JSON.parse(localStorage.getItem(props.inpt.id)).text
const label = "오늘의 할일을 입력하세요."
const inputText2 = ref("")
/* 새로고침후 인풋값 안받아와지는 버그 해결 */
if(inputText == label){
    inputText2.value = ""
} else{
    inputText2.value = inputText
}

const fnInputText = function() {   
    if(inputText2.value.length > 0){
        inputText = inputText2.value
        const todoItem = {
            id: props.inpt.id,
            text: inputText,
            checked: checked.value,
            date: new Date().getDate
        }
        localStorage.setItem(todoItem.id,JSON.stringify(todoItem))
    } else{
        inputText = "오늘의 할일을 입력하세요."
        const todoItem = {
            id: props.inpt.id,
            text: inputText,
            checked: checked.value,
            date: new Date().getDate
        }
        localStorage.setItem(todoItem.id,JSON.stringify(todoItem))
    }
}
/* checkBox */
const checked = ref(props.inpt.checked)

const classObject = reactive({
    text_deco: checked
})

/* checked 속성 localStorage에 변경될때마다 저장 */
watch(checked, (newChecked) => {
    const todoItem = {
        id: props.inpt.id,
        text: inputText,
        checked: newChecked,
        date: new Date().getDate
    }
    localStorage.setItem(todoItem.id,JSON.stringify(todoItem))
})

</script>
