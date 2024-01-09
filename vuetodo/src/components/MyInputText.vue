<template>
    <div class="button_box div_solid">
        <input type="checkbox" class="form-check-input input_box" v-model="checked"/>
        <input v-model="inputText2" ref="inptBox" v-if = "isInputRef" class="form-control maple-light input"  placeholder="오늘의 할일을 입력하세요." @focusout="fnInputDetail($event)" @keyup.enter="fnInputDetail()" v-focus/> 
        <text :class="classObject" v-else class="maple-light input" @dblclick="fnInputDetail()">{{ inputText }}</text>
    </div>
</template>

<script setup>
// import MyInput from './MyInput.vue'
import {ref, watch, reactive} from 'vue';
import {defineProps} from 'vue';

    const props = defineProps({      
        isInput: {
            type: Boolean,   
            required: true, 
            default: false
        },                     
        inpt: {
            id: {
                type: Number,
                required: true
            } 
        }
    }) 
    watch(props, (newValue) => { 
        fnChangeInputRef(newValue.isInputRef);
    })

    const isInputRef = ref(props.isInput)

    const fnInputDetail = function(e) {   

        console.log(e)
        isInputRef.value = !isInputRef.value;  
        fnInputText()

    }

    const fnChangeInputRef = (value)=> {   
        isInputRef.value = value
    };  
    /* onFocus */
    const vFocus = {
        mounted: (el) => el.focus()
    }

    /* text */
    const inputText = ref(props.inpt.text)

    const inputText2 = ref("")

    const fnInputText = function() {   
        if(inputText2.value.length > 0){
            inputText.value = inputText2.value
        } else{
            inputText.value = props.inpt.text
        }
    }
    const checked = ref()
    const classObject = reactive({
        text_deco: checked
    })
</script>
