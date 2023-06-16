<template>
    <div class="container custom_body_bg p-3 mt-4">
        <section>
            <h1 class="text-center">Calender</h1>
        </section>
        <h2>{{currentMonthName}} {{currentYear}}</h2>
        <section>
            <div class="days d-flex">
                <p v-for="(day,index) in days" :key="index" class="text-center font-weight-bold">{{ day }}</p>
            </div>
        </section>
        <section>
            <div class="dates d-flex">
                <p class="text-center" v-for="day in startDayOfMonth" :key="day"></p>
                <p v-for="date in totalDaysOfMonth" :key="date" class="text-center">
                    <!-- <span class="date" :class="date===todaysDate && todaysMonth==currentMonthInNumber?todaysDateBgColor:''">{{ date }}</span> -->
                    <span class="date" :class="todaysDate(date)" @click="getDate">{{ date }}</span>
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
<script>
export default {
    data(){
        return{
            days:["Sun","Mon","Tue","Wed","Thu","Fri","Sat"],
            currentMonthInNumber: new Date().getMonth(),
            currentYear: new Date().getFullYear(),
            currentDate: new Date().getDate(),
        }
    },
    computed:{
        currentMonthName(){
            return new Date(this.currentYear, this.currentMonthInNumber, this.currentDate).toLocaleString('default', { month: 'long' });
        },
        totalDaysOfMonth(){
            return new Date(this.currentYear, this.currentMonthInNumber+1,0).getDate();
        },
        startDayOfMonth(){
            return new Date(this.currentYear, this.currentMonthInNumber,1).getDay();
        }
    },
    methods:{
        prev(){
            if(this.currentMonthInNumber===0){
                this.currentMonthInNumber=11;
                this.currentYear--;
            }else{
                this.currentMonthInNumber--;
            }
            console.log(this.currentMonthInNumber)
        },
        next(){
            if(this.currentMonthInNumber===11){
                this.currentMonthInNumber=0;
                this.currentYear++;
            }else{
                this.currentMonthInNumber++;
            }
        },
        todaysDate(date){
            let calenderDate = new Date(this.currentYear,this.currentMonthInNumber,date).toDateString();
            let today = new Date().toDateString();
            return calenderDate===today?'bg-primary text-light border border-5 p-2':'';
        },
    }
}
</script>
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
        padding: 0.5rem !important;
        border: 3px solid #ffffff;
    }
</style>