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
                        <v-row><label> Ingresa el Nombre del grupo</label></v-row>
                        <v-row> <v-text-field required title="Este campo es requerido" v-model="item.name" background-color="#F5F5F5" shaped dense solo flat class="input-category" label="Nombre" /> </v-row>
                        <v-row><label> Seleccioona los campos para este grupo</label></v-row>
                        <v-row>
                            <v-select v-model="item.attributes" :items="setItems" item-text="name" item-value="name" chips label="Selecciona" multiple solo ></v-select>
                        </v-row>
                        <v-row><label> Ingresa la ponderación de este grupo </label></v-row>
                        <v-row align="center"> 
                            <v-text-field type="tel" id="telefono" maxlength="3" label="0" hide-details required title="Este campo es requerido" v-model="item.percentage" background-color="#F5F5F5" shaped dense solo flat class="input-category input-percentage" /> 
                            <span class="pl-1">%</span>
                            <v-spacer/>
                            <v-btn color="error"  @click="$emit('deleteCategory', index)"> Eliminar grupo </v-btn>
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
                    <v-btn color="success" @click="setResults()" >Continuar</v-btn>
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
    },
    methods: {
        setResults() {
            console.log('DATA: ', this.categories)
            var test = 0;
            for( var category = 0; category < this.categories.length; category++ ) {
                test = 0;
                for(var attribute = 0; attribute < this.categories[category].attributes.length; attribute++) {
                    console.log( parseFloat(this.studentsData.data[0][this.categories[category].attributes[attribute]] ))
                    test += parseFloat(this.studentsData.data[0][this.categories[category].attributes[attribute]])
                }
                test = Math.round( (test / this.categories[category].attributes.length) * ( parseInt(this.categories[category].percentage) * 0.01) );
                // test = (test / this.categories[category].attributes.length);
                console.log("Test: ", test)
            }
            
            
        }
    }
}
</script>

<style scoped>
    .input-category { border-radius:10px; }
    .input-percentage { max-width: 50px; }
</style>