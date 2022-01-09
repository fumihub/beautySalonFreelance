<template>
  <v-app>
    <NavigationMenu :color="color" :flat="flat" />
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer padless>
      <v-row justify="center" no-gutters class="py-3">
        <v-col class="text-center" cols="12">
          <span>&copy; {{ new Date().getFullYear() }} 美容師フリーランス</span>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
import NavigationMenu from '../components/NavigationMenu.vue'
export default {
  name: 'DefaultLayout',
  components: { NavigationMenu },
  data() {
    return {
      fixed: false,
      color: '',
      flat: null,
    }
  },
}
</script>
