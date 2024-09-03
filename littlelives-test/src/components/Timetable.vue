<script setup lang="ts">
import {ref,reactive,onMounted,unref,computed} from "vue";
import { TimeTable, type TimeTableItem, type TimeTableLocation } from 'vue3-timetable';
import Dialog from 'primevue/dialog';
import ScheduleForm from "./ScheduleForm.vue"
// {
//     id: "e3",
//     locationId: 2,
//     startDate: `2024-07-09T14:00:00`,
//     endDate: `2024-07-09T16:00:00`,
//     name: "Surprise Event",
// }

    // {
    //     id: 1,
    //     name: "Mainstage",
    //     items: [
    //         {
    //             id: "e1",
    //             startDate: `2024-07-09T08:00:00`,
    //             endDate: `2024-07-09T11:00:00`,
    //             name: "Main Event",
    //             info: "Don't miss it!",
    //             style: {
    //                 backgroundColor: "#999",
    //                 color: "#000",
    //             },
    //         },
    //     ],
    // },

const state = reactive({
  items: [] as TimeTableItem[],
  days: [] as TimeTableLocation[],
  currDate: new Date(),
  visible:false
});

onMounted(()=>{
  appendNextDays()
})

const getDay = (key) => {
  switch (key) {
    case 0:
      return "Sun"
      break;
    case 1:
      return "Mon"
      break;
    case 2:
      return "Tues"
      break;
    case 3:
      return "Wed"
      break;
    case 4:
      return "Thurs"
      break;
    case 5:
      return "Fri"
      break;
    case 6:
      return "Sat"
      break;
    default:
      return ""
      break;
  }
}

const appendNextDays = () => {
  const today = state.currDate;

  for (let i = 0; i < 7; i++) {
    const futureDate = new Date(today);
    futureDate.setDate(today.getDate() + i);


    state.days.push({
      id: `day_${i}`,
      // startDate: futureDate.toISOString().split('T')[0] + 'T14:00:00',
      // endDate: futureDate.toISOString().split('T')[0] + 'T16:00:00',
      name: `${futureDate.getDate()} ${getDay(futureDate.getDay())}`,
      items:[],
    });
  }
  
  // state.days = [...addDays];
};


</script>

<template>
  <div class="h-screen">
    <div class="text-right py-4">
      <button type="button" class="rounded-full bg-sky-400 py-2	px-3 text-white " @click="state.visible = true">Add Schedule</button>
    </div>
    <TimeTable variant="vertical" :items="state.items" :locations="state.days" :starting-hour="6" :number-of-hours="15"/>

    
  </div>
  <Dialog v-model:visible="state.visible" modal header="Schedule" :style="{ width: '80%' }">
    <ScheduleForm />
  </Dialog>
</template>

<style scoped>

</style>
