<template>
  <div>
    <v-app-bar
      :color="color"
      :flat="flat"
      class="px-15"
      :class="{ expand: flat }"
    >
      <v-toolbar-title>
        <MainIcon />
      </v-toolbar-title>
      <v-spacer />
      <v-app-bar-nav-icon
        v-if="isXs"
        class="mr-4"
        @click.stop="drawer = !drawer"
      />
      <div v-else class="my-2">
        <v-btn rounded class="ml-4" @click="$vuetify.goTo('#hero')"
          ><span class="text-center">使い方</span>
        </v-btn>
        <v-btn rounded class="ml-4" @click="$vuetify.goTo('#hero')"
          ><span class="text-center">ログイン</span>
        </v-btn>
        <v-btn rounded class="ml-4" @click="$vuetify.goTo('#hero')"
          ><span class="text-center">新規登録</span>
        </v-btn>
      </div>
    </v-app-bar>
  </div>
</template>

<script>
import MainIcon from './MainIcon.vue'
export default {
  props: {
    color: { type: String, required: true, default: 'primary' },
    flat: Boolean,
  },
  data: () => ({
    isXs: false,
    fixed: null,
  }),
  watch: {
    isXs(value) {
      if (!value) {
        if (this.drawer) {
          this.drawer = false
        }
      }
    },
  },
  mounted() {
    this.onResize()
    window.addEventListener('resize', this.onResize, { passive: true })
  },
  methods: {
    onResize() {
      this.isXs = window.innerWidth < 850
    },
  },
  components: { MainIcon },
}
</script>

<style scoped>
.v-toolbar {
  transition: 0.6s;
}

.expand {
  height: 80px !important;
  padding-top: 10px;
}
</style>
