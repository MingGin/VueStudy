<template>
    <v-container>
      <v-row justify="space-around">
        <v-menu
        :close-on-content-click="false"
        min-width="auto"
        >
            <template v-slot:activator="{ props }">
                <v-text-field
                v-model="dateFrommat"
                label="toDay's Todo"
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
        </v-row>
    </v-container>
</template>

<script setup>
import { onBeforeMount, ref, watch } from 'vue'
import {defineEmits} from 'vue'
import dayjs from 'dayjs'

const emit = defineEmits(['todoDate'])
const date = ref(new Date(dayjs()))
const dateFrommat = ref(date.value.toLocaleDateString())
const subDate = new Date(dayjs().subtract(8,'day'))

onBeforeMount(() => {
  emit('todoDate',new Date(dayjs()))
});

watch(date, (newDate) => {
    dateFrommat.value = newDate.toLocaleDateString()
})

</script>