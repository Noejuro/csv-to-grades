<template>
    <v-row id="site-footer" justify="center" align="center">
        <img @click="$router.push({ path: '/' })" src="https://cdn.glitch.com/c9340de0-d9c1-4c8a-988c-2836b8426a55%2FImagen1.png?v=1621648108779" class="logo-footer" alt="logo">
    </v-row>
</template>

<style scoped>
    #site-footer {
        background-color: #463f57;
        box-shadow: 0px 2px 10px #888888;
        transition: 1s;
        z-index: 1;
        height: 130px;
        position: relative;
    }
    .logo-footer { width: 60px; max-width: 60px; height: auto; max-height: 100%; cursor: pointer;}
</style>