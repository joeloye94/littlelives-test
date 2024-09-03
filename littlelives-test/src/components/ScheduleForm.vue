<script setup lang="ts">
import {ref,reactive,onMounted,unref,computed} from "vue";
import Select from 'primevue/select';
import InputText from 'primevue/inputtext';
import Checkbox from 'primevue/checkbox';
import DatePicker from 'primevue/datepicker';


// const availableTimesOpt = [
//   "7:00am", "7:30am", "8:00am", "8:30am", "9:00am",
//   "9:30am", "10:00am", "10:30am", "11:00am", "11:30am",
//   "12:00pm", "12:30pm", "1:00pm", "1:30pm", "2:00pm",
//   "2:30pm", "3:00pm", "3:30pm", "4:00pm", "4:30pm",
//   "5:00pm", "5:30pm", "6:00pm", "6:30pm", "7:00pm",
//   "7:30pm", "8:00pm"
// ];

const visitOpt = [
  {
    label:"15 min",
    value:15
  },
  {
    label:"30 min",
    value:30
  },
  {
    label:"45 min",
    value:45
  },
  {
    label:"60 min",
    value:60
  },
  {
    label:"90 min",
    value:90
  }
];

const noOfBookingOpt = [
  {
    label:"1",
    value:1
  },
  {
    label:"2",
    value:2
  },
  {
    label:"3",
    value:3
  },
  {
    label:"4",
    value:4
  },
];



const state = reactive({
    visitDuration:{
        label:"15 min",
        value:15
    },
    noOfBooking:{
        label:"1",
        value:1
    },
    isAllowVideoCall:false,
    schedules:[
        {
            day:"Mon",
            availableTimes:[],
            available:false
        },
        {
            day:"Tues",
            availableTimes:[],
            available:false
        },
        {
            day:"Wed",
            availableTimes:[],
            available:false
        },
        {
            day:"Thurs",
            availableTimes:[],
            available:false
        },
        {
            day:"Fri",
            availableTimes:[],
            available:false
        },
        {
            day:"Sat",
            availableTimes:[],
            available:false
        },
        {
            day:"Sun",
            availableTimes:[],
            available:false
        }
    ]
});

onMounted(()=>{
})

const updateTimeList = (ev, v) => {
    if(ev){
        v.availableTimes.push({
            startTime:null,
            endTime:null
        })
    }else{
        v.availableTimes = []
    }
}

const updateEndTime = (v, min) => {
    const startTime = new Date(v.startTime)
    const endTime = startTime.setTime(startTime.getTime() + (min * 1000 * 60))
    v.endTime = new Date(endTime);
}

const updateAllTimeSlots = () => {
    //update time slot if visit duration is udpated
}

const saveSchedule = () => {

}
</script>

<template>
  
    <form class="space-y-4">
      <div>
        <label for="name" class="block text-sm font-medium text-gray-700">Visit Duration</label>
        <Select v-model="state.visitDuration" @update:modelValue="updateAllTimeSlots" :options="visitOpt" optionLabel="label" optionsValue="value" placeholder="Select" class="w-full md:w-56" />
      </div>

      <div>
        <label for="email" class="block text-sm font-medium text-gray-700">No. of Booking/Session</label>
        <Select v-model="state.noOfBooking" :options="noOfBookingOpt" optionLabel="label" optionsValue="value" placeholder="Select" class="w-full md:w-56" />
      </div>

      <div>
        <div class="flex items-center">
            <Checkbox v-model="state.isAllowVideoCall" inputId="vidCall" name="vidCall" :binary="true" />
            <label for="vidCall" class="ml-2"> Allow Video Tour Call </label>
        </div>
      </div>

      <hr>

      <div>
        <h3 class="font-bold">Availablity</h3>
        <p>Set your weekly recurring schedule</p>
        
        <div v-for="(v,k) in state.schedules" :key="k">
            <div class="flex justify-between py-4 items-center">
                <div>
                    <Checkbox @update:modelValue="(ev)=>updateTimeList(ev, v)" v-model="v.available" :binary="true" />
                    {{ v.day }}
                </div>
                <div v-if="v.available">
                    <div v-for="(_v, _k) in v.availableTimes" class="flex justify-between items-center">
                        <DatePicker v-model="_v.startTime" @update:modelValue="updateEndTime(_v, state.visitDuration.value)" timeOnly placeholder="Start Time"/>
                        <div class="align-middle mx-2"> - </div>
                        <DatePicker v-model="_v.endTime" timeOnly placeholder="End Time" disabled/>
                        
                        <i class="pi pi-plus cursor-pointer ml-3" style="font-size: 1rem"></i>

                    </div>
                    <!-- <Select v-model="v.startTime" :options="availableTimesOpt" placeholder="Select Time" class="w-full md:w-56" />
                    <Select v-model="v.endTime" :options="availableTimesOpt" placeholder="Select Time" class="w-full md:w-56" /> -->
                </div>
                <div v-else>Unavailable</div>
            </div>
            <hr>
        </div>
      </div>


      <div class="flex justify-end">
        <button @click="saveSchedule" type="submit" class="inline-flex items-center py-2 px-3 rounded-full text-white bg-sky-400">
          Save
        </button>
      </div>
    </form>
  
</template>

<style scoped>

</style>
