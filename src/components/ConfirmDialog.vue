<template>
  <v-row justify="center">
    

    <v-dialog
      v-model="showdialog"
      max-width="290"
    
    >
      <v-card>
        <v-card-title class="headline">
          {{title}}
        </v-card-title>

        <v-card-text>
          {{message}}
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn v-if="showcancel"
            color="green darken-1"
            text
            @click="$emit('cancelconfirmdialog')"
          >
            {{cancellabel}}
          </v-btn>

          <v-btn
            color="green darken-1"
            text
            @click="$emit('agreeconfirmdialog')"
          >
            {{agreelabel}}
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
  export default {
      name: "ConfirmDialog",
      props:['message','title','showdialog','cancellabel','agreelabel','showcancel'],
    data () {
      return {
        dialog: true,
      }
    },
  }
</script>
