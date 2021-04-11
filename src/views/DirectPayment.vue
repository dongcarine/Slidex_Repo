<template>
    <Payment :servicerequestid="requestid"/>
    
</template>
<script lang="ts">
import Vue from 'vue'

import Payment from '../components/Payment.vue'
export default Vue.extend({
     data: () => ({
         requestid:null,
     }),
     mounted(){
         
         
     },
     created(){
         console.log("Service Id:"+this.$route.params.serviceid)
         this.requestid=this.$route.params.serviceid
     },
     components:{
         Payment
          
     }
})
</script>