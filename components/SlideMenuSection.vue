<template>
    <section id="carouselPicture">
      <v-container fluid>
          <v-row align="center" justify="center">
            <v-col cols="12" sm="10" md="9" lg="7" xl="6">
              <v-tabs
                v-model="tab"
                grow
              >
                <v-tabs-slider color="yellow"></v-tabs-slider>
                <v-tab
                  v-for="topMenu in topMenus"
                  :key="topMenu"
                >
                  {{ topMenu }}
                </v-tab>
              </v-tabs>
              <v-tabs-items v-model="tab">
                <v-tab-item
                  v-for="topMenu in topMenus"
                  :key="topMenu"
                >
                  <v-card v-if="topMenu === 'トップ'">
                    <h1 class="top-intro-title pa-1">FullNameについて</h1>
                    <v-card>
                      <v-card-title class="font-weight-bold">
                        title
                      </v-card-title>
                      <v-card-text>
                        
                      </v-card-text>
                    </v-card>
                    <v-card>
                      <v-card-title class="font-weight-bold">
                        FullNameの予約情報
                      </v-card-title>
                      <client-only>
                        <swiper class="swiper" style="margin: 20px;" :options="swiperOption">
                          <swiper-slide
                            v-for="(reserve,i) in reserve"
                            :key="i"
                            reverse-transition="fade-transition"
                            transition="fade-transition">
                            <v-card class="my-2">
                              <div class="area font-weight-bold py-2">
                                <p v-html="reserve.date" class="inline-block"></p>
                                <br>
                                <p v-html="reserve.availableText" class="inline-block"></p>
                                <br>
                                <v-btn v-if="reserve.available === true" class="btn inline-block">予約</v-btn>
                              </div>
                            </v-card>
                          </swiper-slide>
                        </swiper>
                      </client-only>
                    </v-card>
                  </v-card>
                  <v-card v-else-if="topMenu === 'メニュー'">
                    <h1 class="top-intro-title pa-1">メニュー</h1>
                    <v-card
                      v-for="(menu,i) in menu"
                      :key="i">
                      <v-card-title class="font-weight-bold">
                        <p>{{ menu.title }}</p>
                      </v-card-title>
                      <v-card-subtitle>
                        <p>{{ menu.text }}</p>
                      </v-card-subtitle>
                      <v-card-text>
                        <p>{{ menu.time }}</p>
                      </v-card-text>
                      <v-card-actions class="area">
                        <v-btn class="btn">予約</v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-card>
                  <v-card v-else-if="topMenu === '口コミ'">
                    <h1 class="top-intro-title pa-1">口コミ</h1>
                    <v-card
                      v-for="(evaluation,i) in evaluation"
                      :key="i">
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
                        <p>{{ evaluation.date }} {{ evaluation.name }} {{ evaluation.age }} ({{ evaluation.gender }})</p>
                      </v-card-subtitle>
                      <v-card-text>
                        <p>{{ evaluation.text }}</p>
                      </v-card-text>
                    </v-card>
                  </v-card>
                  <v-card v-else-if="topMenu === 'アクセス'">
                    <h1 class="top-intro-title pa-1">アクセス</h1>
                    <v-card
                      v-for="(map,i) in map"
                      :key="i">
                      <v-card-title class="font-weight-bold">
                        <p>{{ map.address }}</p>
                      </v-card-title>
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
    name: 'swiper-example-multiple-slides-per-biew',
    title: 'Centered slides',
    components: {
      Swiper,
      SwiperSlide
    },
    data () {
      const reserveDate = new Date();
      const dayOfWeekStr = [ "日", "月", "火", "水", "木", "金", "土" ];
      const reserves = [];
      const menus = [];
      const evaluations = [];
      const maps = [];
      for (let i = 0; i < 31; i++) {
        const preMonth = reserveDate.getMonth()+1;
        if (i !== 0) {
          reserveDate.setDate(reserveDate.getDate() + 1);
        }
        // 1日追加した月と追加する前の月が異なる場合
        if (preMonth !== reserveDate.getMonth()+1) {
          break;
        }
        const reserve = {};
        // 土曜日の場合
        if (dayOfWeekStr[reserveDate.getDay()] === '土') {
          // 青文字に修正
          reserve.date = `<span style="color:#0000ff;">${reserveDate.getMonth()+1}/${reserveDate.getDate()}(${dayOfWeekStr[reserveDate.getDay()]})</span>`;
        }
        // 日曜日の場合
        else if (dayOfWeekStr[reserveDate.getDay()] === '日') {
          // 赤文字に修正
          reserve.date = `<span style="color:#ff0000;">${reserveDate.getMonth()+1}/${reserveDate.getDate()}(${dayOfWeekStr[reserveDate.getDay()]})</span>`;
        }
        // 土日曜日の場合
        else {
          reserve.date = `${reserveDate.getMonth()+1}/${reserveDate.getDate()}(${dayOfWeekStr[reserveDate.getDay()]})`;
        }
        reserve.available = true;
        if (reserve.available) {
          reserve.availableText = `<span style="color:#ff0000;">○</span>`;
        }
        else {
          reserve.availableText = `<span style="color:#0000ff;">×</span>`;
        }
        reserves.push(reserve);
      }
      for (let i = 0; i < 10; i++) {
        const menu = {};
        menu.title = 'カットメニュー';
        menu.text = '詳細テキスト';
        menu.price = `¥${i}`;
        menu.time = `時間：${i}分`
        menus.push(menu);
      }
      const evaluationDate = new Date();
      for (let i = 0; i <= 5; i+=0.5) {
        const evaluation = {};
        evaluation.date = `${evaluationDate.getMonth()+1}/${evaluationDate.getDate()}(${dayOfWeekStr[evaluationDate.getDay()]})`;
        evaluation.name = 'name';
        evaluation.age = i;
        evaluation.gender = '女性';
        evaluation.rate = i;
        evaluation.title = '口コミ';
        evaluation.text = '口コミ内容';
        evaluations.push(evaluation);
      }
      for (let i = 0; i <= 1; i++) {
        const map = {};
        map.address = '東京都'
        maps.push(map);
      }
      return {
        swiperOption: {
          slidesPerView: 4,
          centeredSlides: true,
          spaceBetween: 10,
          loop: false,
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          }
        },
        tab: null,
        topMenus: [
          'トップ', 'メニュー', '口コミ', 'アクセス'
        ],
        reserve: reserves,
        menu: menus,
        evaluation: evaluations,
        map: maps,
        maplocation: { lng: 0, lat: 0 },
        zoom: 4,
        styleMap: {
          width: '100%',
          height: '400px',
        },
        mapOptions: {
          streetViewControl: false,
          styles: [],
        },
      }
    },
  }
</script>

<style lang="scss" scoped>

.top-intro-title{
  background-color: #C0C0C0;
}

.area{
  text-align:center;

  & .btn{
    background:#FAE290;
    padding:3px 1em 3px 1em;
  }
}

.inline-block{
    display:inline-block;
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
