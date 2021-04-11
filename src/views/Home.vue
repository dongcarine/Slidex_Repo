<template>
   <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/icon.png')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3" style="color:#3e0e69">
          Welcome to SlideX
        </h1>

        <p class="subheading font-weight-regular">
           SlideX Direct Payment Portal
         
        </p>
      </v-col>
   </v-row>
   
</template>

<script>
// @ is an alias to /src


export default {
  name: 'Home',
  components: {
    
  }
}
</script>
