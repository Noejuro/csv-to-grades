<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pb-6 pt-0 px-0 FontRegular">
                <v-row justify="center" align="center" class="title-background" style="color: white">
                    <v-col cols="2"></v-col>
                    <v-col cols="8"> <v-row class="text-center FontBold" justify="center"> Selecciona todos los campos a considerar, como actividades o exámenes, para la calificación final </v-row> </v-col>
                    <v-col cols="2"> <v-row justify="end"> <v-icon large color="white" @click="$emit('closeDialogAttributes')" style="cursor: pointer"> mdi-close </v-icon> </v-row> </v-col>
                </v-row>
                <v-row class="py-4 px-6">
                    <v-chip v-for="(item, index) in attributes" class="ma-2" :class="(item.selected) ? 'enabled-chip' : 'disabled-chip' " :key="index" @click="setAttributeStatus(index)" :color="(item.selected) ? '#5f9ea0' : '' " >
                        <strong>{{ item.name }}</strong>
                    </v-chip>
                </v-row>
                <v-row justify="space-between" class="px-6">
                    <v-btn color="#5f9ea0" plain @click="$emit('backToDataTable')">Regresar</v-btn>
                    <button @click="$emit('goToSetPercentages')" class="csv-btn" > Continuar </button>
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
    methods: {
        setAttributeStatus(index) {
            this.attributes[index].selected = !this.attributes[index].selected;
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

<style scoped>
    .disabled-chip { color: #4C4C4C; transition: 0.3s; }
    .enabled-chip { color: white; transition: 0.3s; }
</style>