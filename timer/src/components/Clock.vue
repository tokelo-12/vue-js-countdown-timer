<template>
    <div class="clock-container">
        <div class="segment-container">
            <div class="clock">{{displayDays}}</div>
            <div class="label"><h2>Days</h2></div>
        </div>
        <div class="segment-container">
            <div class="clock">{{displayHours}}</div>
            <div class="label"><h2>Hours</h2></div>
        </div>
        <div class="segment-container">
            <div class="clock">{{displayMinutes}}</div>
            <div class="label"><h2>Minutes</h2></div>
        </div>
        <div class="segment-container">
            <div class="clock">{{displaySeconds}}</div>
            <div class="label"><h2>Seconds</h2></div>
        </div> 
    </div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue';

    
    let displayDays = ref(0)
    let displayHours = ref(0)
    let displayMinutes = ref(0)
    let displaySeconds = ref(0)


    const _seconds = computed(()=> 1000)
    const _minutes= computed(()=> _seconds.value*60)
    const _hours = computed(()=> _minutes.value*60) 
    const _days = computed(()=> _hours.value*24)
    
    onMounted(()=>{
        showRemaining()
    })

    function format(num){
        return num < 10 ? "0" + num : num
    }
    
    const showRemaining = () =>{

        const timer = setInterval(()=>{
            const now = new Date();
            const end = new Date(2024,0,1,0,0,0,0); //end date at 1st jan 2024 00:00:00:00
            const distance = end.getTime() - now.getTime();
            
            if(distance < 0 ){
                clearInterval(timer)
                return
            }

            const days = Math.floor(distance/_days.value);
            const hours = Math.floor((distance % _days.value)/_hours.value)
            const minutes = Math.floor((distance % _hours.value)/_minutes.value)
            const seconds = Math.floor((distance % _minutes.value)/_seconds.value)
            
            //preceding 0 format
            displaySeconds.value = format(seconds)
            displayMinutes.value = format(minutes);
            displayHours.value = format(hours);
            displayDays.value = format(days);
        }, 1000)
    }


</script>