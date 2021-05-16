<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pa-6">
                <v-row justify="center" class="text-center"> Estos son los alumnos que se han cargado desde el archivo</v-row>
                <v-row justify="center" class="text-center pt-2">  ¿Deseas continuar? </v-row>
                <v-row justify="center" class="py-4">
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
                <v-row justify="space-around">
                    <v-btn color="info" @click="$emit('closeDialog')">Cancelar</v-btn>
                    <v-btn color="success" @click="$emit('goToSelectAttributes')">Aceptar</v-btn>
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
    created() {
        if (process.env.NODE_ENV == 'development')
            console.log("HEADERS: ", this.headers)
    },
    methods: {
        
    }
}
</script>

<style scoped>

</style>