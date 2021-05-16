<template>
    <v-dialog v-model="dialog" persistent max-width="1000">
        <v-card>
            <v-col class="blue2 pa-6">
                <v-row justify="end">
                    <v-col cols="4"> <v-row class="text-center" justify="center"> Selecciona todas las actividades o exámenes a considerar </v-row> </v-col>
                    <v-col cols="4"> <v-row justify="end"> <v-icon medium color="black" @click="$emit('closeDialogAttributes')" style="cursor: pointer"> mdi-close </v-icon> </v-row> </v-col>
                </v-row>
            </v-col>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: {dialog: Boolean, studentsData: Object },
}
</script>