<script setup>
    import { reactive,computed,ref,watch } from 'vue'

    const data = reactive({
        days:["Sun","Mon","Tue","Wed","Thu","Fri","Sat"],
        currentMonthInNumber: new Date().getMonth(),
        currentYear: new Date().getFullYear(),
        currentDate: new Date().getDate(),
    })
    
    const selected_date = ref(0);
    const startdate = ref(0);
    const enddate = ref(0);

    const currentMonthName = computed(()=>{
        return new Date(data.currentYear, data.currentMonthInNumber, data.currentDate).toLocaleString('default', { month: 'long' });
    });

    const totalDaysOfMonth = computed(()=>{
        return new Date(data.currentYear, data.currentMonthInNumber+1,0).getDate();
    });

    const startDayOfMonth = computed(()=>{
        return new Date(data.currentYear, data.currentMonthInNumber,1).getDay();
    });
    
    
    function prev(){
        if(data.currentMonthInNumber===0){
            data.currentMonthInNumber=11;
            data.currentYear--;
        }else{
            data.currentMonthInNumber--;
        }
        // console.log(this.currentMonthInNumber)
    }

    function next(){
        if(data.currentMonthInNumber===11){
            data.currentMonthInNumber=0;
            data.currentYear++;
        }else{
            data.currentMonthInNumber++;
        }
    }
    function todaysDate(date){
        let calenderDate = new Date(data.currentYear,data.currentMonthInNumber,date).toDateString();
        let today = new Date().toDateString();
        return calenderDate===today?'bg-primary text-light border border-5':'';
    }

    function selectDate(event){
        selected_date.value = event.target.innerHTML;
    }
    function dblselectDate(){
        selected_date.value = 0;
    }

    function selectedDate(date){
        // console.log(typeof(date))
        // console.log('start'+typeof(startdate.value))
        // console.log('end'+typeof(enddate.value))


        if(date==enddate.value){
            return 'bg-secondary text-light border border-5';
        }
        if(startdate.value>0 && startdate.value<enddate.value){
            return date>=startdate.value && date<=enddate.value?'bg-secondary text-light border border-5 ':'';
        }
    }

    watch(selected_date,
        (newValue, oldValue) => {
            // console.log("old:"+ oldValue)
            // console.log("New:"+ newValue)
            startdate.value = Number(oldValue);
            enddate.value = Number(newValue);

            // if(oldValue>newValue){
            //     multiselect_class.value='';
            // }else{
            //     multiselect_class.value= 'bg-secondary text-light border border-5 p-2' 
            // }
        }
    )

</script>
<template>
    <div class="container custom_body_bg p-3 mt-4">
        <section>
            <h1 class="text-center">Calender</h1>
        </section>

        <h2>{{currentMonthName}} {{data.currentYear}}</h2>
        <section>
            <div class="days d-flex">
                <p v-for="(day,index) in data.days" :key="index" class="text-center font-weight-bold">{{ day }}</p>
            </div>
        </section>
        <section>
            <div class="dates d-flex">
                <p class="text-center" v-for="day in startDayOfMonth" :key="day"></p>
                <p v-for="date in totalDaysOfMonth" :key="date" class="text-center" :class="selectedDate(date)" @click="selectDate" @dblclick="dblselectDate">
                    <span class="date" :class="todaysDate(date)" >{{ date }}</span>
                </p>
            </div>
        </section>
        <section>
            <div class="d-flex justify-content-between">
                <button class="btn btn-primary" @click="prev">Previous</button>
                <button class="btn btn-primary" @click="next">Next</button>
            </div>
        </section>
    </div>
</template>

<style>
    .days p, .dates p{
        width:14.28%;
    }
    .dates{
        flex-wrap: wrap;
    }
    .custom_body_bg{
        background-color: #e0e7ea
    }

    .date:hover{
        border: 3px solid #ffffff;
    }
</style>