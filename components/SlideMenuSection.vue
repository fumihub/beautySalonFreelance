<template>
  <section id="carouselPicture">
    <v-container fluid>
      <v-row align="center" justify="center">
        <v-col cols="12" sm="10" md="9" lg="7" xl="6">
          <v-tabs v-model="tab" grow>
            <v-tabs-slider color="yellow"></v-tabs-slider>
            <v-tab v-for="topMenu in topMenus" :key="topMenu">
              {{ topMenu }}
            </v-tab>
          </v-tabs>
          <v-tabs-items v-model="tab">
            <v-tab-item v-for="topMenu in topMenus" :key="topMenu">
              <v-card v-if="topMenu === 'トップ'">
                <h1 class="top-intro-title pa-1">
                  {{ lastName + ' ' + firstName }}について
                </h1>
                <v-card>
                  <v-card-text>
                    {{ introduction }}
                  </v-card-text>
                </v-card>
                <v-card>
                  <v-card-title class="font-weight-bold">
                    {{ lastName + ' ' + firstName }}の予約情報
                  </v-card-title>
                  <client-only>
                    <swiper
                      class="swiper"
                      style="margin: 20px"
                      :options="swiperOption"
                    >
                      <swiper-slide
                        v-for="(reserve, i) in reserve"
                        :key="i"
                        reverse-transition="fade-transition"
                        transition="fade-transition"
                      >
                        <v-card class="my-2">
                          <div class="area font-weight-bold py-2">
                            <p class="inline-block" v-html="reserve.date"></p>
                            <br />
                            <p
                              class="inline-block"
                              v-html="reserve.availableText"
                            ></p>
                            <br />
                            <v-btn
                              v-if="reserve.available === true"
                              class="btn inline-block"
                              >予約</v-btn
                            >
                          </div>
                        </v-card>
                      </swiper-slide>
                    </swiper>
                  </client-only>
                </v-card>
              </v-card>
              <v-card v-else-if="topMenu === 'メニュー'">
                <h1 class="top-intro-title pa-1">メニュー</h1>
                <v-card v-for="(menu, i) in menus" :key="i">
                  <v-card-title class="font-weight-bold">
                    <p>{{ menu.menu }}</p>
                  </v-card-title>
                  <v-card-subtitle>
                    <p>{{ menu.menuDetail }}</p>
                  </v-card-subtitle>
                  <v-card-text>
                    <p>{{ `¥${menu.price}` }}</p>
                  </v-card-text>
                  <v-card-text>
                    <p>{{ `時間：${menu.timeSelect}` }}</p>
                  </v-card-text>
                  <v-card-actions class="area">
                    <v-btn class="btn">予約</v-btn>
                  </v-card-actions>
                </v-card>
              </v-card>
              <v-card v-else-if="topMenu === '口コミ'">
                <h1 class="top-intro-title pa-1">口コミ</h1>
                <v-card v-for="(evaluation, i) in evaluation" :key="i">
                  <v-card-title class="font-weight-bold">
                    <p>{{ evaluation.title }}</p>
                  </v-card-title>
                  <v-rating
                    :value="evaluation.rate"
                    color="amber"
                    dense
                    half-increments
                    readonly
                    size="14"
                    class="px-3"
                  ></v-rating>
                  <v-card-subtitle>
                    <p>
                      {{ evaluation.date }} {{ evaluation.name }}
                      {{ evaluation.age }} ({{ evaluation.gender }})
                    </p>
                  </v-card-subtitle>
                  <v-card-text>
                    <p>{{ evaluation.text }}</p>
                  </v-card-text>
                </v-card>
              </v-card>
              <v-card v-else-if="topMenu === 'SNS'">
                <h1 class="top-intro-title pa-1">SNS</h1>
                <v-card
                  v-for="(sns, i) in sns"
                  :key="i"
                  :color="sns.color"
                  dark
                >
                  <v-card-title>
                    <v-icon large left>
                      {{ sns.icon }}
                    </v-icon>
                    <span class="text-h6 font-weight-light">{{
                      sns.title
                    }}</span>
                  </v-card-title>
                  <v-card-text class="font-weight-bold">
                    <p>{{ snsURL[i] }}</p>
                  </v-card-text>
                </v-card>
              </v-card>
            </v-tab-item>
          </v-tabs-items>
        </v-col>
      </v-row>
    </v-container>
  </section>
</template>

<script>
import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
import 'swiper/css/swiper.css'
export default {
  name: 'SwiperExampleMultipleSlidesPerBiew',
  title: 'Centered slides',
  components: {
    Swiper,
    SwiperSlide,
  },
  props: {
    lastName: {
      type: String,
      required: true,
    },
    firstName: {
      type: String,
      required: true,
    },
    menus: {
      type: Array,
      required: true,
    },
    introduction: {
      type: String,
      required: true,
    },
    snsURL: {
      type: Array,
      required: true,
    },
  },
  data() {
    // 本日の日付取得
    const reserveDate = new Date()
    const dayOfWeekStr = ['日', '月', '火', '水', '木', '金', '土']
    const reserves = []
    const evaluations = []
    const MAX_DAYS_OF_MONTH = 31
    // 予約情報
    for (let i = 0; i < MAX_DAYS_OF_MONTH; i++) {
      // 初回ループ以外
      if (i !== 0) {
        // 日付を1日プラス
        reserveDate.setDate(reserveDate.getDate() + 1)
      }
      const reserve = {}
      // 土曜日の場合
      if (dayOfWeekStr[reserveDate.getDay()] === '土') {
        // 青文字に修正
        reserve.date = `<span style="color:#0000ff;">${
          reserveDate.getMonth() + 1
        }/${reserveDate.getDate()}(${
          dayOfWeekStr[reserveDate.getDay()]
        })</span>`
      }
      // 日曜日の場合
      else if (dayOfWeekStr[reserveDate.getDay()] === '日') {
        // 赤文字に修正
        reserve.date = `<span style="color:#ff0000;">${
          reserveDate.getMonth() + 1
        }/${reserveDate.getDate()}(${
          dayOfWeekStr[reserveDate.getDay()]
        })</span>`
      }
      // 土日以外の場合
      else {
        reserve.date = `${
          reserveDate.getMonth() + 1
        }/${reserveDate.getDate()}(${dayOfWeekStr[reserveDate.getDay()]})`
      }
      // 予約できるかチェック
      reserve.available = true
      if (reserve.available) {
        reserve.availableText = `<span style="color:#ff0000;">○</span>`
      } else {
        reserve.availableText = `<span style="color:#0000ff;">×</span>`
      }
      reserves.push(reserve)
    }
    // 口コミ情報
    const evaluationDate = new Date()
    for (let i = 0; i <= 5; i += 0.5) {
      const evaluation = {}
      evaluation.date = `${
        evaluationDate.getMonth() + 1
      }/${evaluationDate.getDate()}(${dayOfWeekStr[evaluationDate.getDay()]})`
      evaluation.name = 'name'
      evaluation.age = i
      evaluation.gender = '女性'
      evaluation.rate = i
      evaluation.title = '口コミ'
      evaluation.text = '口コミ内容'
      evaluations.push(evaluation)
    }
    return {
      swiperOption: {
        slidesPerView: 4,
        centeredSlides: true,
        spaceBetween: 10,
        loop: false,
        pagination: {
          el: '.swiper-pagination',
          clickable: true,
        },
      },
      tab: null,
      topMenus: ['トップ', 'メニュー', '口コミ', 'SNS'],
      reserve: reserves,
      evaluation: evaluations,
      sns: [
        {
          icon: 'mdi-facebook',
          title: 'Facebook',
          color: '#3b5998',
        },
        {
          icon: 'mdi-twitter',
          title: 'Twitter',
          color: '#26c6da',
        },
        {
          icon: 'mdi-instagram',
          title: 'Instagram',
          color: '#E1306C',
        },
      ],
    }
  },
}
</script>

<style lang="scss" scoped>
.top-intro-title {
  background-color: #c0c0c0;
}

.area {
  text-align: center;

  & .btn {
    background: #fae290;
    padding: 3px 1em 3px 1em;
  }
}

.inline-block {
  display: inline-block;
}

.swiper-slide {
  width: 60%;
}
.swiper-slide:nth-child(2n) {
  width: 40%;
}
.swiper-slide:nth-child(3n) {
  width: 20%;
}
</style>
