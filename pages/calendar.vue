<script setup>
import FullCalendar from "@fullcalendar/vue3";
import dayGridPlugin from "@fullcalendar/daygrid";
import interactionPlugin from "@fullcalendar/interaction";
const nuxtApp = useNuxtApp();

const calendarOptions = ref(
    {
        plugins: [dayGridPlugin, interactionPlugin],
        initialView: 'dayGridMonth',
        events: [
            { title: "event 1", date: "2023-02-20" },
            { title: "event 2", date: "2023-02-23" },
        ],
    }
)


nuxtApp.vueApp.component("FullCalendar", FullCalendar)


let events = ref([
    {
        date: {
            year: 2023,
            month: 2,
            date: 20
        },
        description: "Go to the gym"
    },
    {
        date: {
            year: 2023,
            month: 2,
            date: 22
        },
        description: "Go cook"
    }
])

onMounted(() => {
    document.querySelector(".fc-header-toolbar").children[1].innerHTML = "<h1>Familien Kalendar</h1>"
})
</script>
<template>
    <div class="p-5">
        <FullCalendar :options='calendarOptions' />
    </div>
</template>