<template>
    <v-row>
        <v-col class="pa-0">
            <v-snackbar :timeout="8000" v-model="snackbar" absolute top right rounded="pill" color="error" elevation="10"> 
               <v-row justify="center">{{snackbarMessage}}</v-row>
            </v-snackbar>
            <dialogTable :dialog="dialog" :studentsData="dataFromFile" :headers="headers" @closeDialog="closeDialog()" @goToSelectAttributes="goToSelectAttributes()" />
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
        </v-col>
    </v-row>
</template>

<script>
   import Papa from 'papaparse'
   import dialogTable from '@/components/dialogTable'
   export default {
      layout: 'main',
      components: { dialogTable },
      data() {
         return {
            fileRecords: [],
            snackbar: false,
            snackbarMessage: '',
            dialog: false,
            dataFromFile: {
               data: null,
               fields: null
            },
            headers: [],
         };
      },
      methods: {
         filesSelected(fileRecordsNewlySelected) {
            if(this.fileRecords[0].ext == 'csv') {
               if (process.env.NODE_ENV == 'development')
                  console.log("File", this.fileRecords[0].file);

               var that = this;

               Papa.parse(this.fileRecords[0].file, {
                  header: true,
                  complete: function (results) {
                        that.setData(results);
                        that.headers = that.getHeaders();
                        that.dialog = true;
                  }
               });
            } else {
               this.fileRecords = [];
               this.snackbar = true;
               this.snackbarMessage = 'El archivo debe ser .CSV';
            }
         },
         onBeforeDelete(fileRecord) {
            this.fileRecords = [];
         },
         fileDeleted(fileRecord) {
            this.fileRecords = [];
         },
         setData(results) {
            if(results.data[0]['Dirección de correo'] == "" && results.data[1]['Dirección de correo'] == "" )
               results.data.splice(0, 2);
            if(!results.data[results.data.length - 1]['Dirección de correo'])
               results.data.splice(results.data.length - 1, 1);

            this.dataFromFile.data = results.data;
            this.dataFromFile.fields = results.meta.fields;

            if (process.env.NODE_ENV == 'development')
               console.log("DATA FILE VUE: ", this.dataFromFile);
         },
         getHeaders() {
            var headers = [];
            for(var i = 0; i < 3; i++ )
                headers.push({ text: this.dataFromFile.fields[i], value: this.dataFromFile.fields[i] });

            if (process.env.NODE_ENV == 'development')
               console.log('HEADERS: ', headers)
            return headers;
        },
        closeDialog() {
           this.dialog = false;
           this.fileRecords = [];
        },
        goToSelectAttributes() {
           this.$router.push({ path: '/selecciona' });
           this.dialog = false;
           this.fileRecords = [];
        }
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

