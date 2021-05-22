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
                    <v-col cols="12" class="border-bottom pt-4"  v-for="(item, index) in categories" :key="index" style="min-height: 200px;">
                        <v-row><label> Ingresa el Nombre de esta categoría</label></v-row>
                        <v-row> <v-text-field required hide-details title="Este campo es requerido" v-model="item.name" background-color="#F5F5F5" shaped dense solo flat class="input-category" label="Nombre" /> </v-row>
                        <v-row class="pb-2">
                            <v-checkbox :ripple="false" dense v-model="item.examen" label="¿Esta categoría es un exámen?" color="red" hide-details ></v-checkbox>
                        </v-row>
                        <v-row v-if="item.examen">
                            <v-col class="pa-0">
                                <v-row><label> Ingresa la puntuación máxima del exámen </label></v-row>
                                <v-row align="center"> 
                                    <v-text-field type="tel" maxlength="3" label="0" hide-details required title="Este campo es requerido" v-model="testScore" background-color="#F5F5F5" shaped dense solo flat class="input-category input-percentage" /> 
                                </v-row>
                            </v-col>
                        </v-row>
                        <v-row><label> Selecciona los campos para esta categoría</label></v-row>
                        <v-row>
                            <v-select v-model="item.attributes" :items="setItems" @change="$emit('setAverageCounter', index)" item-text="name" item-value="name" chips label="Selecciona" multiple solo ></v-select>
                        </v-row>
                        <v-row v-if="!item.examen" align="center">
                            <label> El promedio de este grupo será obtenido dividiendo entre </label>
                            <v-text-field type="tel" maxlength="2" label="0" hide-details required title="Este campo es requerido" v-model="item.averageCounter" background-color="#F5F5F5" shaped dense solo flat class="input-category input-averageCounter centered-input RotoplasBold" />
                            <label> actividades </label>
                        </v-row>
                        <v-row><label> Ingresa la ponderación de esta categoría </label></v-row>
                        <v-row align="center"> 
                            <v-text-field type="tel" maxlength="3" label="0" hide-details required title="Este campo es requerido" v-model="item.percentage" background-color="#F5F5F5" shaped dense solo flat class="input-category input-percentage centered-input" /> 
                            <span class="pl-1">%</span>
                            <v-spacer/>
                            <v-btn color="error"  @click="$emit('deleteCategory', index)"> Eliminar categoría </v-btn>
                        </v-row>
                    </v-col>
                    <v-col cols="12">
                        <v-row style="min-height: 50px; cursor: pointer; border: 2px dashed cadetblue;" @click="$emit('addCategory')" align="center" class="text-center" justify="center">
                            Da clic aquí para agregar un nuevo grupo
                        </v-row>
                    </v-col>
                </v-row>
                <v-row justify="space-between">
                    <v-btn color="blue" plain @click="$emit('backToAttributes')">Regresar</v-btn>
                    <v-btn color="success" @click="$emit('setResults', testScore)" >Continuar</v-btn>
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
            testScore: '',
        }
    },
    computed: {
        setItems() {
            return this.studentsData.fields.filter(activity => activity.selected == true)
        }
    },
}
</script>

<style scoped>
    .input-category { border-radius:10px; }
    .input-percentage { max-width: 50px; }
    .input-averageCounter { max-width: 50px; }
    .input-averageCounter >>> input { font-weight: 700; }
    .centered-input >>> input {
      text-align: center
    }
</style>