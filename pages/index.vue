<template>
    <v-row>
        <v-col class="pa-0">
            <v-snackbar :timeout="8000" v-model="snackbar" absolute top right rounded="pill" color="error" elevation="10"> 
               <v-row justify="center">{{snackbarMessage}}</v-row>
            </v-snackbar>
            <v-row justify="center"> Upload your file </v-row>
            <client-only>
               <v-row id="profile-pic-demo" justify="center">
                  <VueFileAgent
                     class="profile-pic-upload-block"
                     ref="profilePicRef"
                     :multiple="false"
                     :deletable="true"
                     :compact="true"
                     :accept="'.csv'"
                     :helpText="'Arrastra aquí tu archivo .CSV, o da clic aquí para seleccionarlo'"
                     :errorText="{
                        type: 'Porfavor sube un archivo .CSV',
                     }"
                     @select="filesSelected($event)"
                     @beforedelete="onBeforeDelete($event)"
                     @delete="fileDeleted($event)"
                     v-model="fileRecords"
                  >
                  </VueFileAgent>
               </v-row>
               <v-row justify="center">
                  <v-btn color="success" @click="uploadFiles()">Subir archivo</v-btn>
               </v-row>
            </client-only>
        </v-col>
    </v-row>
</template>

<script>
  export default {
    layout: 'main',
    data() {
      return {
         fileRecords: [],
         snackbar: false,
         snackbarMessage: '',
      };
    },
    methods: {
      uploadFiles: function () {
         if(this.fileRecords[0].ext == 'csv') {
            console.log("File", this.fileRecords);
         } else {
            this.fileRecords = [];
            this.snackbar = true;
            this.snackbarMessage = 'El archivo debe ser .CSV';
         }
      },
      filesSelected: function (fileRecordsNewlySelected) {
         if(this.fileRecords[0].ext == 'csv') {
            console.log("File", this.fileRecords);
         } else {
            this.fileRecords = [];
            this.snackbar = true;
            this.snackbarMessage = 'El archivo debe ser .CSV';
         }
      },
      onBeforeDelete: function (fileRecord) {
        this.fileRecords = [];
      },
      fileDeleted: function (fileRecord) {
        this.fileRecords = [];
      },
   },
};
</script>

<style scoped>
   .selector-text { color: blue; font-size: 15px; }
</style>

<style>
   #profile-pic-demo .drop-help-text { display: none; }
   #profile-pic-demo .is-drag-over .drop-help-text { display: block; }
   #profile-pic-demo .profile-pic-upload-block { border: 2px dashed transparent; padding: 20px; padding-top: 0; width: 90%; max-width: 500px; }
   #profile-pic-demo .is-drag-over.profile-pic-upload-block { border-color: #AAA; }
   #profile-pic-demo .vue-file-agent { width: 100%; float: left; border: 0; box-shadow: none; }
</style>

