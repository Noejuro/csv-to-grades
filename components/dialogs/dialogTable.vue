<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pb-6 pt-0 px-0 FontRegular">
                <v-row justify="center" align="center" class="title-background" style="color: white">
                    <v-col cols="2"></v-col>
                    <v-col cols="8"> <v-row class="text-center FontBold" justify="center"> Estos son los alumnos que se han cargado desde el archivo </v-row> </v-col>
                    <v-col cols="2"> <v-row justify="end"> <v-icon large color="white" @click="$emit('closeDialogDataTable')" style="cursor: pointer"> mdi-close </v-icon> </v-row> </v-col>
                </v-row>
                <v-row justify="center" class="py-4 px-6">
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
                </v-row>
                <v-row justify="center" class="px-6">
                    <button @click="$emit('goToSelectAttributes')" class="csv-btn" style="width: 100%"> Continuar </button>
                </v-row>
            </v-col>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: {dialog: Boolean, studentsData: Object, headers: Array },
    data() {
        return {
            students: [],
            search: ''
        }
    },
    methods: {
        
    }
}
</script>

<style scoped>

</style>