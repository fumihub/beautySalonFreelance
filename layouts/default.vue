<template>
  <v-app>
    <NavigationMenu :color="color" :flat="flat" />
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <FooterMenu />
  </v-app>
</template>

<script>
import NavigationMenu from '../components/NavigationMenu.vue'
import FooterMenu from '../components/FooterMenu.vue'
export default {
  name: 'DefaultLayout',
  components: { NavigationMenu, FooterMenu },
  data() {
    return {
      fixed: false,
      color: '',
      flat: null,
    }
  },
}
</script>
