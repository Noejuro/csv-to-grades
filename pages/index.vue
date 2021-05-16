<template>
    <v-row style="background-color: #5a9698" class="py-10">
        <v-col class="pa-0">
            <v-snackbar :timeout="8000" v-model="snackbar" absolute top right rounded="pill" color="error" elevation="10"> 
               <v-row justify="center">{{snackbarMessage}}</v-row>
            </v-snackbar>
            <dialogTable :dialog="dialogDataTable" :studentsData="dataFromFile" :headers="headers" @closeDialogDataTable="closeDialogDataTable()" @goToSelectAttributes="goToSelectAttributes()" />
            <dialogAttributes :dialog="dialogSelectAttributes" :studentsData="dataFromFile" @closeDialogAttributes="closeDialogAttributes()" @backToDataTable="backToDataTable" @goToSetPercentages="goToSetPercentages"/>
            <dialogSetPercentages :dialog="dialogSetPercentages" :studentsData="dataFromFile" :categories="categories" @addCategory="addCategory" @deleteCategory="deleteCategory" @closeDialogSetPercentages="closeDialogSetPercentages()" @backToAttributes="backToAttributes" />
            <v-row justify="center" class="py-10"> Sube tu archivo para comenzar </v-row>
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
   import dialogAttributes from '@/components/dialogAttributes'
   import dialogSetPercentages from '@/components/dialogSetPercentages'
   export default {
      layout: 'main',
      components: { dialogTable, dialogAttributes, dialogSetPercentages },
      data() {
         return {
            fileRecords: [],
            snackbar: false,
            snackbarMessage: '',
            dialogDataTable: false,
            dialogSelectAttributes: false,
            dialogSetPercentages: false,
            dataFromFile: {
               data: null,
               fields: []
            },
            headers: [],
            categories: [
               {
                  name: '',
                  attributes: [],
                  percentage: 0
               }
            ]
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
                        that.dialogDataTable = true;
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

            var fields = [];

            for(var i = 0; i < results.meta.fields.length; i++ )
               fields.push({ name: results.meta.fields[i], selected: false })

            this.dataFromFile.data = results.data;
            this.dataFromFile.fields = fields;

            if (process.env.NODE_ENV == 'development')
               console.log("DATA FILE VUE: ", this.dataFromFile);
         },
         getHeaders() {
            var headers = [];
            for(var i = 0; i < 3; i++ )
                headers.push({ text: this.dataFromFile.fields[i].name, value: this.dataFromFile.fields[i].name });

            if (process.env.NODE_ENV == 'development')
               console.log('HEADERS: ', headers)
            return headers;
         },
         closeDialogDataTable() {
            this.dialogDataTable = false;
            this.fileRecords = [];
         },
         closeDialogAttributes() {
            this.dialogSelectAttributes = false;
            this.fileRecords = [];
         },
         closeDialogSetPercentages() {
            this.dialogSetPercentages = false;
            this.fileRecords = [];
         },
         goToSelectAttributes() {
            this.dialogDataTable = false;
            this.dialogSelectAttributes = true;
         },
         goToSetPercentages() {
            this.dialogSelectAttributes = false;
            this.dialogSetPercentages = true;
         },
         backToDataTable() {
            this.dialogSelectAttributes = false;
            this.dialogDataTable = true;
         },
         backToAttributes() {
            this.dialogSelectAttributes = true;
            this.dialogSetPercentages = false;
         },
         addCategory() {
            this.categories.push({
               name: '',
               attributes: [],
               percentage: 0
            })
         },
         deleteCategory(index) {
            this.categories.splice(index, 1);
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
   #profile-pic-demo .profile-pic-upload-block { border: 2px dashed white; background-color: cadetblue; padding: 0; color: white; width: 90%; max-width: 600px; }
   #profile-pic-demo .is-drag-over.profile-pic-upload-block { border-color: transparent; }
   #profile-pic-demo .vue-file-agent { width: 100%; float: left; border: 0; box-shadow: none; }
   .file-preview svg { fill: white !important; margin-top: 30% !important; height: 26% !important }
   .file-preview span { color: white !important; border-color: transparent; font-size: 25px !important; line-height: normal !important; height: auto !important; }
   .vue-file-agent .file-preview-new { padding: 0 !important; }
   .vue-file-agent .file-preview .file-delete { top: 5px; right: 15px !important }
   .vue-file-agent .is-deletable .file-preview .file-name { padding-top: 10px }
</style>

