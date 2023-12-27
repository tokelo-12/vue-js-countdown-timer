<template>
    <div class="clock-container">
        <!-- <div class="segment-container">
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
        </div> -->
        
        <div class="countdown">
            <div class="time-section" id="days">
                <div class="time-group">
                    <div class="time-segment">
                        <div class="segment-display">
                            <div class="segment-display-top">{{displayDays}}</div>
                            <div class="segment-display-bottom">{{displayDays}}</div>
                            <div class="segment-overlay" :class="{flip: isActive}">
                                <div class="segment-overlay-top">{{displayDays}}</div>
                                <div class="segment-overlay-bottom">{{displayDays}}</div>
                            </div>
                        </div>
                    </div>
                </div>
                <p>days</p>
            </div>

            <div class="time-section" id="hours">
                <div class="time-group">
                    <div class="time-segment">
                        <div class="segment-display">
                            <div class="segment-display-top">{{displayHours}}</div>
                            <div class="segment-display-bottom">{{displayHours}}</div>
                            <div class="segment-overlay" :class="{flip: isActive}">
                                <div class="segment-overlay-top">{{displayHours}}</div>
                                <div class="segment-overlay-bottom">{{displayHours}}</div>
                            </div>
                        </div>
                    </div>
                </div>
                <p>hours</p>
            </div>

            <div class="time-section" id="minutes">
                <div class="time-group">
                    <div class="time-segment">
                        <div class="segment-display">
                            <div class="segment-display-top">{{displayMinutes}}</div>
                            <div class="segment-display-bottom">{{displayMinutes}}</div>
                            <div class="segment-overlay" :class="{flip: isActive}">
                                <div class="segment-overlay-top">{{displayMinutes}}</div>
                                <div class="segment-overlay-bottom">{{displayMinutes}}</div>
                            </div>
                        </div>
                    </div>
                </div>
                <p>minutes</p>
            </div>


            <div class="time-section" id="seconds">
                <div class="time-group">
                    <div class="time-segment" >
                        <div class="segment-display">
                            <div class="segment-display-top">{{displaySeconds.toString().padStart(2, '0')}}</div>
                            <div class="segment-display-bottom">{{displaySeconds.toString().padStart(2, '0')}}</div>
                            <div class="segment-overlay" :class="{flip: shouldFlip}">
                                <div class="segment-overlay-top">{{displaySeconds.toString().padStart(2, '0')}}</div>
                                <div class="segment-overlay-bottom">{{displaySeconds.toString().padStart(2, '0')}}</div>
                            </div>
                        </div>
                    </div>   
                </div>
                <p>seconds</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed, onMounted, ref, defineProps } from 'vue';

    const props = defineProps({
        year:{
            type: Number,
            required: true,
        },
        month:{
            type: Number,
            required: true,
        },
        date:{
            type: Number,
            required: true,
        },
        hour:{
            type: Number,
            required: true,
        },
        minute:{
            type: Number,
            required: true,
        },
        second:{
            type: Number,
            required: true,
        },
        millisecond:{
            type: Number,
            required: true,
        }
    })

    
    let displayDays = ref(0)
    let displayHours = ref(0)
    let displayMinutes = ref(0)
    let displaySeconds = ref(0)
    let isActive = ref(false)

    const _seconds = computed(()=> 1000)
    const _minutes= computed(()=> _seconds.value*60)
    const _hours = computed(()=> _minutes.value*60) 
    const _days = computed(()=> _hours.value*24)
    const end = computed(()=> new Date(
        props.year,
        props.month,
        props.date,
        props.hour,
        props.minute,
        props.second,
        props.millisecond,
        ))
    
    onMounted(()=>{
        showRemaining()
    })

    function format(num){
            return num.toString().padStart(2,'0');
        
    }

    const shouldFlip = (x)=>{
      return x % 2 === 0; // Toggle on even seconds
    }
    
    const showRemaining = () =>{

        // //update display and overlay elements at the same time
        // const updateSegmentValues = (overlayElement, displayElement, value)=>{
        //     displayElement = value;
        //     overlayElement = value;
        // }

        // const updateTimeSegment = (segmentElement, timeValue)=>{
        //     //TODO get access to segmentElement
            
        //     //add flip class for animation
        //     !isActive.value

        //     updateSegmentValues(
        //         segmentElement
        //     )
        // }

        const timer = setInterval(()=>{
            const now = new Date();
            //const end = new Date(2024,0,1,0,0,0,0); //end date at 1st jan 2024 00:00:00:00
            const distance = end.value.getTime() - now.getTime();
            
            if(distance < 0 ){
                clearInterval(timer)
                return
            }

            const days = Math.floor(distance/_days.value);
            const hours = Math.floor((distance % _days.value)/_hours.value)
            const minutes = Math.floor((distance % _hours.value)/_minutes.value)
            const seconds = Math.floor((distance % _minutes.value)/_seconds.value)
            
            
            displaySeconds.value = seconds;
            displayMinutes.value = format(minutes);
            displayHours.value = format(hours);
            displayDays.value = format(days);
            shouldFlip(displaySeconds)
            !shouldFlip(displaySeconds)
            // !isActive.value


        }, 1000)
    }


</script>