<template>
    <v-container>
        <v-row justify="space-around">
            <v-col
            cols="12"
            md="6"
            >
                <v-menu
                :close-on-content-click="false"
                min-width="auto"
                >
                    <template v-slot:activator="{ props }">
                        <v-text-field
                        v-model="dateFrommat"
                        label="Today's Todo"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="props"
                        >
                        </v-text-field>
                    </template>
                    <v-date-picker
                    v-model="date"
                    scrollable
                    show-adjacent-months
                    color="primary"
                    hide-header="true"
                    rounded="xl"
                    @click="$emit('todoDate',date)"
                    :min="subDate"
                    >
                    </v-date-picker>
                </v-menu>
            </v-col>
            <v-col 
            cols="12"
            md="6">
                <TodoComplete :count = "compleCnt" ></TodoComplete>
            </v-col>
        </v-row>
    </v-container>
</template>

<script setup>
import TodoComplete from './TodoComplete.vue';
import { onBeforeMount, ref, toRef, watch } from 'vue'
import {defineEmits, defineProps} from 'vue'
import dayjs from 'dayjs'

const props = defineProps({                  
    count: {
        type: Number,
        required: true,
        default: 0
    }
})

const emit = defineEmits(['todoDate'])
const date = ref(new Date(dayjs()))
const dateFrommat = ref(date.value.toLocaleDateString())
const subDate = new Date(dayjs().subtract(8,'day'))
const compleCnt = toRef(props.count)

watch (
  () => props.count,
  (count) => {
    compleCnt.value = count
    console.log(compleCnt.value)
    }
)


onBeforeMount(() => {
  emit('todoDate',new Date(dayjs()))
});

watch(date, (newDate) => {
    dateFrommat.value = newDate.toLocaleDateString()
})

</script>