

<template>



  <div class="container mx-auto ">
    <p class="text-md text-gray-500 font-serif pt-">
      Select the units, number, and frequency for HTTP API requests based on expected volume. The calculations below exclude free tier discounts.</p>
      <p>HTTP API requests units</p>
      <p>The unit will apply to the number of requests you will specify below.</p>
      
      <select class="
      selectoption
      form-select form-select-lg 
      mt-3 mb-3
      focus:outline-non
      
      
      
      " @change="switchSelect($event)">
        <option value="">Choose</option>
        <option value=1>exact number</option>
        <option value=1000>thousands</option>
        <option value=1000000>millions</option>
        
      </select>
      <div class="flex space-x-2 mt-3">
      <input type="number" class="px-6 py-2 h-8 border mt-1 rounded-md" v-model="requests" name="" id="">
      <select class="selectoption form-select form-select-lg focus:outline-none" @change="switchSelectSecond($event)">
        <option value="">Choose</option>
        <option value=2628000>per second</option>
        <option value=43800>per minute</option>
        <option value=730>per hour</option>
        <option value=30.42>per day</option>
        <option value=1>per month</option>
      </select>
      </div>
      <div class="flex flex-col">
        <p>Cache Memory</p>
        <select class="selectoption form-select form-select-lg focus:outline-none" @change="switchcacheSelect($event)">
        <option value="">Choose</option>
        <option value=0.02>0.5</option>
        <option value=0.038>1.6</option>
        <option value=0.20>6.1</option>
        <option value=0.25>13.5</option>
        <option value=0.5>28.4</option>
        <option value=1.0>58.2</option>
        <option value=1.9>118</option>
        <option value=3.8>237</option>
      </select>
      </div>
        Total Requests -{{totalRequests}}  
        
      
      <br>
      <!-- 1 per second * (60 seconds in a minute x 60 minutes in an hour x 730 hours in a month)-{{requests*selected * 60*60*730}} -->
   
      Unit conversion for Api requests 
       Requests: {{requests}} per second * (60 seconds in a minute x 60 minutes in an hour x 730 hours in a month)-{{totalRequests}}
       <p class="text-xs">{{totalRequests}} requests * {{selected}} unit multiplier={{totalRequests}}</p>
       <p>Tiered price for: {{totalRequests}} requests </p>
       <p>2628000 requests x 0.0000035000 USD ={{totalPricebasedonRequests}}USD</p>
       <div v-if="selectedcache">
           <p>{{selectedcache}} USD per hour * 730 hours in a month={{totalCacheCost}} USD for cache memory</p>
       <p>Dedicated Cache memory total price-{{totalCacheCost}}</p>
       <p>{{totalPricewithChache}} USD + {{totalCacheCost}}={{totalPricewithChache}} USD</p>
        <p class="font-bold">REST API cost (monthly):-{{totalPricewithChache}}</p>
       </div>
      
    </div>
</template>
<script setup>
import {ref,computed} from 'vue'
const selected=ref(1);
const selectedsecond=ref('');
const selectedcache=ref('');

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
const switchcacheSelect=(event)=>{
  selectedcache.value=event.target.value
}
// const totalRequestsperSecondnAMonth= computed (()=>{
//   return requests.value * selected.value * 60 * 60 * 730
// })
 
 const totalRequests= computed (()=>{
  return requests.value *selected.value* selectedsecond.value 
})
const totalPricebasedonRequests=computed(()=>{
  let totalReqwithoutCache=requests.value *selected.value* selectedsecond.value;
  let totalRequestsminus333=totalReqwithoutCache-333000000
   let totalReqminusbillion=totalReqwithoutCache-1000000000
   let totalReqminustrillion=totalReqwithoutCache-20000000000
   if(totalReqwithoutCache>20000000000){
     return 333000000 * 0.0000035  + 667000000 * 0.0000028 + 19000000000 * 0.0000023800 + totalReqminustrillion * 0.0000015100 
   }else if(totalReqwithoutCache>1000000000){
        return 333000000 * 0.0000035  + 667000000 * 0.0000028 + totalReqminusbillion *  0.0000023800
  }else if(totalReqwithoutCache>333000000 && totalReqwithoutCache<1000000000){
             return 333000000 * 0.0000035  + totalRequestsminus333 * 0.0000028
  }else{
    return totalReqwithoutCache * 0.0000035
  }
   
 })
const totalCacheCost=computed(()=>{
   return selectedcache.value * 730
})
const totalPricewithChache=computed(()=>{
  let totalReqwithcache=requests.value *selected.value* selectedsecond.value;
  let totalRequestsminus333withcache=totalReqwithcache-333000000
  let totalReqminusbillionwithcache=totalReqwithcache-1000000000
     let totalReqminustrillionwithCache=totalReqwithcache-20000000000
  // if(totalReq>333000000){
  //       return 333000000 * 0.0000035  + totalRequestsminus333 * 0.0000028 + selectedcache.value * 730
  // }else{
  //    return  totalReq * 0.0000035 + selectedcache.value * 730
  // }
  if(totalReqwithcache>20000000000){
     return 333000000 * 0.0000035  + 667000000 * 0.0000028 + 19000000000 * 0.0000023800 
     + totalReqminustrillionwithCache * 0.0000015100 +selectedcache.value * 730
   }else if(totalReqwithcache>1000000000){
        return 333000000 * 0.0000035  + 667000000 * 0.0000028 +  totalReqminusbillionwithcache *  0.0000023800 + selectedcache.value * 730
  }else if(totalReqwithcache>333000000 && totalReqwithoutCache<1000000000){
             return 333000000 * 0.0000035  + totalRequestsminus333 * 0.0000028+selectedcache.value * 730
  }
  
  // return requests.value *selected.value* selectedsecond.value  * 0.0000035 + selectedcache.value * 730
})
</script>