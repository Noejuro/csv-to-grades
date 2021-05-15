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
            </client-only>
            <v-row justify="center">
               Data {{ dataFromFile }}
            </v-row>
        </v-col>
    </v-row>
</template>

<script>
   import Papa from 'papaparse'
   export default {
      layout: 'main',
      data() {
         return {
            fileRecords: [],
            snackbar: false,
            snackbarMessage: '',
            dataFromFile: null,
         };
      },
      methods: {
         filesSelected: function (fileRecordsNewlySelected) {
            if(this.fileRecords[0].ext == 'csv') {
               if (process.env.NODE_ENV == 'development')
                  console.log("File", this.fileRecords[0].file);

               var that = this;

               Papa.parse(this.fileRecords[0].file, {
                  header: true,
                  complete: function (results) {
                        console.log("DATA FILE: ", results);
                        that.dataFromFile = results.data;
                        that.fields = results.fields;
                        console.log("DATA FILE VUE: ", that.dataFromFile);
                  }
               });
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
   #profile-pic-demo .profile-pic-upload-block { border: 0; background-color: cadetblue; padding: 0; color: white; width: 90%; max-width: 600px; }
   #profile-pic-demo .is-drag-over.profile-pic-upload-block { border-color: transparent; }
   #profile-pic-demo .vue-file-agent { width: 100%; float: left; border: 0; box-shadow: none; }
   .file-preview svg { fill: white !important; margin-top: 30% !important; height: 26% !important }
   .file-preview span { color: white !important; border-color: transparent; font-size: 25px !important; line-height: normal !important; height: auto !important; }
   .vue-file-agent .file-preview-new { padding: 0 !important; }
   .vue-file-agent .file-preview .file-delete { top: 5px; right: 15px !important }
   .vue-file-agent .is-deletable .file-preview .file-name { padding-top: 10px }
</style>

