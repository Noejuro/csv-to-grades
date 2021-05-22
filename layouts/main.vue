<template>
    <v-app class="font-regular" style="line-height: normal; font-size: 18px">
      <v-col class="pa-0 FontRegular">
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
  head: {
      title: 'CSV a Calificación',
      meta: [
          { name: 'description', content: 'Web platform which can perform the calculations of all students in a dynamic, simple and personalized way in order to support teachers to reduce time to obtain grades.' },
      ],
  },
  components: { siteHeader, siteFooter }
}
</script>

<style>
    .row { margin: 0px !important; }
    .border-bottom { border-bottom: 1px solid #4C4C4C; }

     /* Importing Fonts */
    @font-face { font-family: 'font_regular'; src: url('https://cdn.glitch.com/c9340de0-d9c1-4c8a-988c-2836b8426a55%2FMontserrat-Regular.ttf?v=1621659606615'); }
    @font-face { font-family: 'font_bold'; src: url('https://cdn.glitch.com/c9340de0-d9c1-4c8a-988c-2836b8426a55%2FMontserrat-SemiBold.ttf?v=1621659607240'); }

    .FontRegular{ font-family: 'font_regular' !important; }
    .FontBold{ font-family: 'font_bold' !important; }

    .Font-title { font-size: 30px; }
    .Font-subtitle { font-size: 20px; }
    .Font-normal { font-size: 18px; }
</style>