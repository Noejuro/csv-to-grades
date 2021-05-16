<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pa-6">
                <v-row justify="center">
                    <v-col cols="2"></v-col>
                    <v-col cols="8"> <v-row class="text-center" justify="center"> Agrupa todas las actividades/exámenes y selecciona la ponderación de cada uno </v-row> </v-col>
                    <v-col cols="2"> <v-row justify="end"> <v-icon medium color="black" @click="$emit('closeDialogSetPercentages')" style="cursor: pointer"> mdi-close </v-icon> </v-row> </v-col>
                </v-row>
                <v-row justify="space-between">
                    <v-btn color="blue" plain @click="$emit('backToAttributes')">Regresar</v-btn>
                    <v-btn color="success" >Continuar</v-btn>
                </v-row>
            </v-col>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: {dialog: Boolean, studentsData: Object },
    data() {
        return {
            attributes: [],
        }
    },
    watch: {
        dialog() {
            if(this.dialog) {
                this.attributes = this.studentsData.fields.slice(3, this.studentsData.fields.length);
            }
        }
    }
}
</script>