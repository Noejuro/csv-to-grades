<template>
    <v-app class="font-regular" style="line-height: normal; font-size: 18px">
      <v-col class="pa-0">
        <siteHeader />
        <Nuxt />
        <siteFooter />
      </v-col>
  </v-app>
</template>

<script>
import siteHeader from '@/components/header'
import siteFooter from '@/components/footer'
export default {
  components: { siteHeader, siteFooter }
}
</script>

<style>
    .row { margin: 0px !important; }
    .border-bottom { border-bottom: 1px solid #4C4C4C; }
</style>