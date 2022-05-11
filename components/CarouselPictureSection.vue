<template>
    <section id="carouselPicture">
      <v-container fluid>
          <v-row align="center" justify="center">
            <v-col cols="12" sm="10" md="9" lg="7" xl="6">
              <v-row align="center" justify="center">
                <v-col cols="12" class="">
                  <client-only>
                    <swiper class="swiper" style="margin: 20px;" :options="swiperOption">
                      <swiper-slide
                        v-for="(item,i) in items"
                        :key="i"
                        reverse-transition="fade-transition"
                        transition="fade-transition">
                        <v-img :src="item.src" :lazy-src="item.src" class="grey lighten-2" />
                      </swiper-slide>
                    </swiper>
                  </client-only>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
      </v-container>
    </section>
</template>

<script>
  import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
  import 'swiper/css/swiper.css'
  export default {
    name: 'SwiperExampleCenteredAuto',
    title: 'Centered slides + Auto slides per view',
    components: {
      Swiper,
      SwiperSlide
    },
    props:{
      images: {
        type: Array,
        required: true
      },
    },
    data () {
      return {
        swiperOption: {
          slidesPerView: 'auto',
          centeredSlides: true,
          spaceBetween: 30,
          loop: true,
          autoplay: {
            delay: 2500,
            disableOnInteraction: false
          },
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          }
        },
        items: [],
      }
    },
    created() {
      // TODO 画像をS3に登録してから取得する
      // for (let i = 0; i < this.images.length; i++) {
      //   this.items.push(URL.createObjectURL(this.images[i].image));
      // }
    },
  }
</script>

<style lang="scss" scoped>
.swiper {
  height: 100%;
  width: 100%; 
}

</style>