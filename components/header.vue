<template>
    <v-row id="site-header" justify="center" align="center">
        <img @click="$router.push({ path: '/' })" src="https://cdn.glitch.com/c9340de0-d9c1-4c8a-988c-2836b8426a55%2FDatos_Alterados__1_-removebg-preview.png?v=1621647241369" class="logo-header" alt="logo">
    </v-row>
</template>

<style scoped>
    #site-header {
        background-color: white;
        box-shadow: 0px 2px 10px #888888;
        transition: 1s;
        z-index: 1;
        max-height: fit-content;
        position: relative;
    }
    .logo-header { width: 130px; max-width: 130px; height: 100%; max-height: 100%; cursor: pointer;}
</style>