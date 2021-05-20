<template>
    <v-row>
        <v-col>
            <v-row justify="center" class="py-4">
                <client-only>
                    <a id="CSVFile"></a>
                    <v-card style="width: 100%">
                        <v-card-title>
                        Alumnos
                        <v-spacer></v-spacer>
                        <v-text-field
                            v-model="search"
                            append-icon="mdi-magnify"
                            label="Buscar"
                            single-line
                            hide-details
                        ></v-text-field>
                        </v-card-title>
                        <v-data-table
                        style="width: 100%"
                        :headers="headers"
                        :items="studentsData.data"
                        class="elevation-1"
                        :search="search"
                        ></v-data-table>
                    </v-card>
                </client-only>
            </v-row>
            <v-row justify="center">
                <v-btn block color="success" @click="downloadCSV()">Descargar Archivo</v-btn>
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
            studentsData: { data: [], fields: [] },
            headers: [],
            search: '',
        }
    },
    mounted() {
        this.studentsData = JSON.parse(localStorage.getItem('StudentsData'));
        this.headers = this.getHeaders();
        if (process.env.NODE_ENV == 'development') {
            console.log("Students: ", this.studentsData)
            console.log("Headers: ", this.headers)
        }
    },
    methods: {
        getHeaders() {
            var headers = [];
            for(var i = 0; i < this.studentsData.fields.length; i++ )
                if(this.studentsData.fields[i].selected || this.studentsData.fields[i].name == 'Apellidos' || this.studentsData.fields[i].name == "Nombre" || this.studentsData.fields[i].name == "Dirección de correo")
                    headers.push({ text: this.studentsData.fields[i].name, value: this.studentsData.fields[i].name });
            return headers;
        },
        async downloadCSV() {
            this.studentsData.fields = this.removeAttributes();
            var csv;
            csv = await Papa.unparse(this.studentsData, {  config: { header: true }  });
            let link = document.getElementById("CSVFile");
            link.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(csv));
            link.setAttribute('download', 'calificaciones.csv');
            console.log(document.getElementById("CSVFile"));
            link.click();
        },
        removeAttributes() {
            var headers = []
            for(var i = 0; i < this.studentsData.fields.length; i++ ) {
                if(this.studentsData.fields[i].selected || this.studentsData.fields[i].name == 'Apellidos' || this.studentsData.fields[i].name == "Nombre" || this.studentsData.fields[i].name == "Dirección de correo") {
                    headers.push(this.studentsData.fields[i].name);
                }
            }
            return headers;
        }
    }
}
</script>

<style scoped>

</style>