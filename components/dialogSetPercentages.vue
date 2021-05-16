<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pa-6">
                <v-row justify="center">
                    <v-col cols="2"></v-col>
                    <v-col cols="8"> <v-row class="text-center" justify="center"> Agrupa todas las actividades/exámenes e ingresa la ponderación de cada uno </v-row> </v-col>
                    <v-col cols="2"> <v-row justify="end"> <v-icon medium color="black" @click="$emit('closeDialogSetPercentages')" style="cursor: pointer"> mdi-close </v-icon> </v-row> </v-col>
                </v-row>
                <v-row>
                    <v-col cols="6"  v-for="(item, index) in categories" :key="index" style="min-height: 200px;">
                        <v-row> <v-text-field required title="Este campo es requerido" v-model="item.name" background-color="#F5F5F5" shaped dense solo flat class="input-category" label="Nombre" /> </v-row>
                        <v-row>
                            
                        </v-row>
                        <v-row align="center"> 
                            <v-text-field type="tel" id="telefono" maxlength="3" hide-details required title="Este campo es requerido" v-model="item.percentage" background-color="#F5F5F5" shaped dense solo flat class="input-category input-percentage" /> 
                            <span class="pl-1">%</span>
                        </v-row>
                        <v-row justify="center"> <v-btn color="error" plain @click="$emit('deleteCategory', index)"> Eliminar </v-btn> </v-row>
                    </v-col>
                    <v-col cols="6">
                        <v-row style="min-height: 200px; cursor: pointer; border: 2px dashed cadetblue;" @click="$emit('addCategory')" align="center" class="text-center" justify="center">
                            Da clic aquí para agregar un nuevo grupo
                        </v-row>
                    </v-col>
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
    props: {dialog: Boolean, studentsData: Object, categories: Array },
    data() {
        return {
        }
    },
    computed: {
        setItems() {
            return this.studentsData.fields.filter(activity => activity.selected == true)
        }
    }
}
</script>

<style scoped>
    .input-category { border-radius:10px; }
    .input-percentage { max-width: 50px; }
</style>