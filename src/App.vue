<template>
  <v-app>
    <v-app-bar
      app
      color="#3e0e69"
      dark
    >
      <div class="d-flex align-center">
      
        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="./assets/logo2.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Download SlideX App</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
     
    </v-main>
     
                             
  </v-app>
</template>

<script>


export default {
  name: 'App',

  components: {
    
  },
  created(){
    
  },
  data: () => ({
    //
  }),
};
</script>
