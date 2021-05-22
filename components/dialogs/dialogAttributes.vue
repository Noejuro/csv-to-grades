<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pa-6">
                <v-row justify="center">
                    <v-col cols="2"></v-col>
                    <v-col cols="8"> <v-row class="text-center" justify="center"> Selecciona todas las actividades o exámenes a considerar </v-row> </v-col>
                    <v-col cols="2"> <v-row justify="end"> <v-icon medium color="black" @click="$emit('closeDialogAttributes')" style="cursor: pointer"> mdi-close </v-icon> </v-row> </v-col>
                </v-row>
                <v-row class="py-4">
                    <v-chip v-for="(item, index) in attributes" class="ma-2" :class="(item.selected) ? 'enabled-chip' : 'disabled-chip' " :key="index" @click="setAttributeStatus(index)" :color="(item.selected) ? 'blue' : '' " >
                        <strong>{{ item.name }}</strong>
                    </v-chip>
                </v-row>
                <v-row justify="space-between">
                    <v-btn color="blue" plain @click="$emit('backToDataTable')">Regresar</v-btn>
                    <v-btn color="success" @click="$emit('goToSetPercentages')">Continuar</v-btn>
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