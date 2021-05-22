<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pb-6 pt-0 px-0 FontRegular">
                <v-row justify="center" align="center" class="title-background" style="color: white">
                    <v-col cols="2"></v-col>
                    <v-col cols="8"> <v-row class="text-center FontBold" justify="center"> Coloca en categorías todos los campos anteriormente seleccionados, e ingresa lo que se te pide de cada uno </v-row> </v-col>
                    <v-col cols="2"> <v-row justify="end"> <v-icon large color="white" @click="$emit('closeDialogSetPercentages')" style="cursor: pointer"> mdi-close </v-icon> </v-row> </v-col>
                </v-row>
                <v-row class="px-6">
                    <v-col cols="12" class="border-bottom py-8"  v-for="(item, index) in categories" :key="index" style="min-height: 200px;">
                        <v-row class="pb-1 pt-2"><label> Ingresa el Nombre de esta categoría</label></v-row>
                        <v-row> <v-text-field style="border: 2px solid cadetblue;" required hide-details title="Este campo es requerido" v-model="item.name" background-color="#F5F5F5" shaped dense solo flat class="input-category" label="Nombre" /> </v-row>
                        <v-row class="pt-2">
                            <v-checkbox :ripple="false" dense v-model="item.examen" label="¿Esta categoría es un exámen?" color="#5f9ea0" hide-details ></v-checkbox>
                        </v-row>
                        <v-row v-if="item.examen">
                            <v-col class="pa-0">
                                <v-row class="pt-0" align="center">
                                    <label class="pr-2"> Ingresa la puntuación máxima del exámen </label>
                                    <v-text-field type="tel" maxlength="3" label="0" hide-details required title="Este campo es requerido" v-model="testScore" background-color="#F5F5F5" shaped dense solo flat class="input-category input-percentage centered-input" /> 
                                    <span class="pl-1">pts</span>
                                </v-row>
                            </v-col>
                        </v-row>
                        <v-row class="pb-1 pt-3"><label> Selecciona los campos para esta categoría</label></v-row>
                        <v-row>
                            <v-select  item-color="#5f9ea0" style="border: 2px solid cadetblue;" v-model="item.attributes" :items="setItems" @change="$emit('setAverageCounter', index)" hide-details item-text="name" item-value="name" chips label="Selecciona" multiple solo >
                                <template #selection="{ item }">
                                    <v-chip color="#463f57" style="color: white">{{item.name}}</v-chip>
                                </template>
                            </v-select>
                        </v-row>
                        <v-row v-if="!item.examen" align="center" class="pb-1 pt-4">
                            <label> El promedio de este grupo será obtenido dividiendo entre </label>
                            <v-text-field type="tel" maxlength="2" label="0" hide-details required title="Este campo es requerido" v-model="item.averageCounter" background-color="#F5F5F5" shaped dense solo flat class="input-category input-activities centered-input RotoplasBold mx-1" />
                            <label> actividades </label>
                        </v-row>
                        <v-row class="pb-1 pt-6" align="center">
                            <label class="pr-2"> Ingresa la ponderación de esta categoría </label>
                            <v-text-field type="tel" maxlength="3" label="0" hide-details required title="Este campo es requerido" v-model="item.percentage" background-color="#F5F5F5" shaped dense solo flat class="input-category input-percentage centered-input" /> 
                            <span class="pl-1">%</span>
                            <v-spacer/>
                            <v-btn color="error" plain @click="$emit('deleteCategory', index)"> Eliminar categoría </v-btn>
                        </v-row>
                    </v-col>
                    <v-col cols="12">
                        <v-row style="min-height: 80px; cursor: pointer; border: 3px dashed cadetblue;" @click="$emit('addCategory')" align="center" class="text-center FontBold Font-subtitle" justify="center">
                            Da clic aquí para agregar una nueva categoría
                        </v-row>
                    </v-col>
                </v-row>
                <v-row  class="px-6 pt-4" justify="space-between">
                    <v-btn color="#5f9ea0" plain @click="$emit('backToAttributes')">Regresar</v-btn>
                    <button @click="$emit('setResults', testScore)" class="csv-btn" > Continuar </button>
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
    .input-percentage { max-width: 50px; border: 2px solid cadetblue; }
    .input-percentage >>> input { font-weight: 700; }
    .input-averageCounter { max-width: 50px; }
    .input-averageCounter >>> input { font-weight: 700; }
    .input-activities { max-width: 50px; border: 2px solid cadetblue; }
    .input-activities >>> input { font-weight: 700; }
    .centered-input >>> input {
      text-align: center
    }
</style>