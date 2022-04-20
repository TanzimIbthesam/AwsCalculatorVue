

<template>



  <div class="container mx-auto">
    <p class="text-md text-gray-500 font-serif pt-">
      Select the units, number, and frequency for HTTP API requests based on expected volume. The calculations below exclude free tier discounts.</p>
      <p>HTTP API requests units</p>
      <p>The unit will apply to the number of requests you will specify below.</p>
      {{selected}}
      <select class="
      mt-3
      form-select form-select-lg mb-3
      appearance-none
      block
      w-1/2
      px-4
      py-1
      text-xl
      font-normal
      text-gray-700
      bg-white bg-clip-padding bg-no-repeat
      border border-solid border-gray-300
      rounded
      transition
      ease-in-out
      m-0
      focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none
      
      
      
      " @change="switchSelect($event)">
        <option value="">Choose</option>
        <option value=1>exact number</option>
        <option value=1000>thousands</option>
        <option value=1000000>millions</option>
        
      </select>
      <div class="flex">
      <input type="number" class="px-6 py-1 h-8 border" v-model="requests" name="" id="">
      <select class="
      mt-3
      form-select form-select-lg mb-3
      appearance-none
      block
      w-1/2
      px-4
      py-1
      text-xl
      font-normal
      text-gray-700
      bg-white bg-clip-padding bg-no-repeat
      border border-solid border-gray-300
      rounded
      transition
      ease-in-out
      m-0
      focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none
      
      
      
      " @change="switchSelectSecond($event)">
        <option value="">Choose</option>
        <option value=2628000>per second</option>
        <option value=43800>per minute</option>
        <option value=730>per hour</option>
        <option value=30.42>per day</option>
        <option value=1>per month</option>
      </select>
      </div>
        SelectedSecond-{{totalRequestsperMinuteandaMonth}}  
      SelectedSecond-{{selectedsecond}}  
      <div v-if="selectedsecond>10000">a lott</div>
      <br>
      <!-- 1 per second * (60 seconds in a minute x 60 minutes in an hour x 730 hours in a month)-{{requests*selected * 60*60*730}} -->
   
      Unit conversion for Api requests 
       Requests: {{requests}} per second * (60 seconds in a minute x 60 minutes in an hour x 730 hours in a month)-{{totalRequestsperSecondnAMonth}}
       <p class="text-xs">{{totalRequestsperSecondnAMonth}} requests * 1 unit multiplier={{totalRequestsperSecondnAMonth}}</p>
       <p>Tiered price for: {{totalRequestsperSecondnAMonth}} requests </p>
       <p>2628000 requests x 0.0000035000 USD ={{priceperMonthbasedonOneSecond}}USD</p>
    </div>
</template>
<script setup>
import {ref,computed} from 'vue'
const selected=ref(1);
const selectedsecond=ref('');
let requests=ref('');
let age=ref(17);
const switchSelect=(event)=>{
  console.log("Event");
  selected.value=event.target.value
  selectedsecond.value=event.target.value
  
}
const switchSelectSecond=(event)=>{
  console.log("Event Second");
  
  selectedsecond.value=event.target.value
  console.log(selectedsecond);
  
}
const totalRequestsperSecondnAMonth= computed (()=>{
  return requests.value * selected.value * 60 * 60 * 730
})
 const priceperMonthbasedonOneSecond=computed(()=>{
   return  requests.value * selected.value * 60 * 60 * 730 * 0.0000035
 })
 const totalRequestsperMinuteandaMonth= computed (()=>{
  return requests.value * selectedsecond.value 
})
</script>