<template>
  <v-footer padless>
    <v-row justify="center" no-gutters class="py-3">
      <v-col class="text-center" cols="12">
        <span>&copy; {{ new Date().getFullYear() }} 美容師フリーランス</span>
      </v-col>
    </v-row>
  </v-footer>
</template>

<script>
export default {
  data: () => ({
    icons: [],
  }),
}
</script>

<style scoped>
.v-card {
  width: 100%;
}
</style>
