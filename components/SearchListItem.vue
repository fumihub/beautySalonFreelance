<template>
  <div :href="link" class="search-list-item-container pb-2">
    <v-row align="start">
      <v-col cols="12">
        <div class="hairdresser-name pl-2">{{ hairdresser.name }}</div>
      </v-col>
    </v-row>
    <v-row align-content="center" justify="center">
      <v-col cols="12" class="hairdresser-swiper py-0">
        <client-only>
          <swiper class="swiper" :options="swiperOption">
            <swiper-slide
              v-for="(item, i) in hairdresser.images"
              :key="i"
              reverse-transition="fade-transition"
              transition="fade-transition"
            >
              <v-img
                :src="item.src"
                :lazy-src="item.src"
                class="grey lighten-2"
              />
            </swiper-slide>
          </swiper>
        </client-only>
      </v-col>
    </v-row>
    <v-row align="start">
      <v-col cols="12" class="ml-2">
        <p class="hairdresser-summary">{{ hairdresser.summary }}</p>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  props: {
    hairdresser: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      link: `#${this.$props.hairdresser.id}`,
      swiperOption: {
        slidesPerView: 3,
        spaceBetween: 8,
      },
    }
  },
}
</script>

<style>
.hairdresser-name {
  font: bold;
  font-size: 1.4rem;
}
.v-img {
  height: 100%;
}
</style>
