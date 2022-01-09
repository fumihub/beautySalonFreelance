<template>
  <div>
    <v-app-bar
      app
      :color="color"
      :flat="flat"
      class="px-15"
      :class="{ expand: flat }"
    >
      <v-toolbar-title>
        <v-row align="center" justify="center">
          <v-col cols="3">
            <svg
              class="mx-6 my-2 py-2"
              width="61"
              height="61"
              viewBox="0 0 61 61"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <circle cx="30.5" cy="30.5" r="30.5" fill="#726548" />
              <path
                d="M53 16.9333C53 16.9333 47.1333 15.1733 42.7333 18.4L30.12 27.2C27.7733 25.44 24.2533 23.3867 20.7333 22.2133V19.8667C20.7333 16.64 18.0933 14 14.8667 14C11.64 14 9 16.64 9 19.8667V24.2667C9 25.7333 10.4667 25.7333 10.4667 25.7333H16.3333C22.2 25.7333 26.6 30.1333 26.6 30.1333C26.6 30.1333 22.2 34.5333 16.3333 34.5333H10.4667C10.4667 34.5333 9 34.5333 9 36V40.4C9 43.6267 11.64 46.2667 14.8667 46.2667C18.0933 46.2667 20.7333 43.6267 20.7333 40.4V38.0533C24.2533 36.88 27.7733 34.8267 30.12 33.0667L42.7333 41.8667C47.1333 45.0933 53 43.3333 53 43.3333L33.9333 30.1333L53 16.9333ZM17.8 22.8H11.9333V19.8667C11.9333 18.1067 13.1067 16.9333 14.8667 16.9333C16.6267 16.9333 17.8 18.1067 17.8 19.8667V22.8ZM17.8 40.4C17.8 42.16 16.6267 43.3333 14.8667 43.3333C13.1067 43.3333 11.9333 42.16 11.9333 40.4V37.4667H17.8V40.4ZM30.2667 31.6C29.8777 31.6 29.5046 31.4455 29.2296 31.1704C28.9545 30.8954 28.8 30.5223 28.8 30.1333C28.8 29.7444 28.9545 29.3713 29.2296 29.0962C29.5046 28.8212 29.8777 28.6667 30.2667 28.6667C30.6557 28.6667 31.0287 28.8212 31.3038 29.0962C31.5788 29.3713 31.7333 29.7444 31.7333 30.1333C31.7333 30.5223 31.5788 30.8954 31.3038 31.1704C31.0287 31.4455 30.6557 31.6 30.2667 31.6Z"
                fill="#F8F8F8"
              />
            </svg>
          </v-col>
          <v-col cols="9">
            <span>美容師フリーランス名鑑</span>
          </v-col>
        </v-row>
      </v-toolbar-title>
      <v-spacer />
      <v-app-bar-nav-icon
        v-if="isXs"
        class="mr-4"
        @click.stop="drawer = !drawer"
      />
      <div v-else>
        <v-btn
          rounded
          color="primary"
          class="ml-4"
          @click="$vuetify.goTo('#hero')"
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
