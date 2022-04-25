<template>
  <v-card class="hairdresser-card my-4 pb-6">
    <div class="top-image-wraper">
      <div class="cover-gradient"></div>
      <img class="hairdresser-main-img" src="@/assets/img/hero.jpg" />
    </div>
    <!-- 美容師の情報 -->
    <v-card-title>
      <div class="hairdresser-name">{{ hairdresser.name }}</div>
    </v-card-title>
    <v-card-text>
      <v-row align="center" class="mx-0">
        <v-rating
          :value="hairdresser.rating"
          color="amber"
          dense
          half-increments
          readonly
          size="20"
        ></v-rating>
        <div class="grey--text ms-4">
          {{ hairdresser.rating }} ({{ hairdresser.ratingCount }})
        </div>
      </v-row>
    </v-card-text>
    <v-card-subtitle class="sub-title">概要</v-card-subtitle>
    <v-card-text>
      {{ hairdresser.summary }}
    </v-card-text>
    <v-card-text>
      {{ hairdresser.id }}
    </v-card-text>
    <v-card-text>
      <v-row align-content="center" justify="center">
        <v-col cols="12" class="hairdresser-swiper px-6">
          <client-only>
            <swiper class="swiper my-2" :options="swiperOption">
              <swiper-slide
                v-for="(item, i) in hairdresser.images"
                :key="i"
                reverse-transition="fade-transition"
                transition="fade-transition"
              >
                <v-img
                  :src="item.src"
                  :lazy-src="item.src"
                  class="slide-content"
                />
              </swiper-slide>
            </swiper>
          </client-only>
        </v-col>
      </v-row>
    </v-card-text>
    <v-row justify="center">
      <v-btn to="link" rounded outlined large class="ma-4">
        詳細を確認する
        <v-icon class="ml-2">mdi-arrow-right</v-icon>
      </v-btn>
    </v-row>
  </v-card>
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
      link: `/users/${this.$props.hairdresser.id}`,
      swiperOption: {
        spaceBetween: 2,
        slidesPerView: 5,
      },
    }
  },
}
</script>

<style scoped>
.cover-gradient {
  position: relative;
  z-index: 100;
  height: 200px;
  background: linear-gradient(0, rgba(255, 255, 255, 0.681) 3%, transparent);
}
.hairdresser-main-img {
  position: absolute;
  top: 0%;
  height: 200px;
  width: 100%;
  border-radius: 4px 4px 0 0;
  object-fit: cover; /* IE: not support */
}
.hairdresser-card {
  position: relative;
}
.hairdresser-name {
  font: bold;
  font-size: 1.4rem;
}
.swiper-container {
  max-height: 100px;
}
.swiper-slide > img {
  object-fit: cover; /* IE: not support */
  width: 100%;
  height: 100%;
}
</style>
