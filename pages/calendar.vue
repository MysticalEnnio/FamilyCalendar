<script setup>
import { NButton, NCalendar, useMessage } from 'naive-ui'

let events = [
    {
        data: "2021-10-01",
    }
]

defineNuxtComponent({
    setup() {
        const message = useMessage();
        return {
            value: ref(new Date()),
            handleUpdateValue(_, { year, month, date }) {
                console.log(`${year}-${month}-${date}`)
                message.success(`${year}-${month}-${date}`);
            },
        }
    }
})
</script>
<template>
    <div class="p-5">
        <h1>Familien Kalender</h1>
        <n-calendar v-model:value="value" #="{ year, month, date }" @update:value="handleUpdateValue()">
            {{ date }}.{{ month }}.{{ year }}
        </n-calendar>
    </div>
</template>