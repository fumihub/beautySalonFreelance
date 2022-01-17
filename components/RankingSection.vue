<template>
  <section id="ranking">
    <v-container fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" class="">
            <h1 align="center" justify="center" class="font-weight-light display-2">人気美容師<br>ランキング</h1>
            <h1 class="font-weight-bold my-3">
              1~10位
            </h1>
            <template>
              <v-carousel
                cycle
                show-arrows
                hide-delimiters
                class="my-3"
              >
                <v-carousel-item
                  v-for="(item,i) in items"
                  :key="i"
                  reverse-transition="fade-transition"
                  transition="fade-transition"
                >
                <v-card style="margin: 20px; ; ;">
                  <v-img v-bind:src="item.src" v-bind:lazy-src="item.src" class="grey lighten-2" />
                  <v-sheet
                    :color="item.backgroundColor"
                    class="circle"
                  >
                    <v-card-text
                      class="text--ranking"
                    >
                      {{ i+1 }}位
                    </v-card-text>
                  </v-sheet>
                  <v-card-title>
                    {{ item.name }}
                  </v-card-title>
                  <v-card-text class="text--primary">
                    {{ item.discription }}
                  </v-card-text>
                </v-card>
              </v-carousel-item>
            </v-carousel>
            </template>
          </v-col>
        </v-row>
    </v-container>
  </section>
</template>

<style scoped>
.circle {
  width: 65px;
  height: 50px;
  border-radius: 50%;
}
.text--ranking {
  color: black;
  font-weight: bold;
  margin-left: 5px;
}

</style>

<script>
  export default {
    data () {
      return {
        items: [
          /* src を v-bind によって変数バインドさせたい場合は
              そのままパス文字列が出力されてしまう。
              回避するために「require()」で囲む
          */ 
          {
            src: require('~/assets/img/1.svg'),
            // 金色
            backgroundColor: '#ffd700',
            name: 'name1',
            discription: "kakak\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka"
          },
          {
            src: require('~/assets/img/2.svg'),
            // 銀色
            backgroundColor: '#c0c0c0',
            name: 'name2',
            discription: '2'
          },
          {
            src: require('~/assets/img/3.svg'),
            // 銅色
            backgroundColor: '#ff8c00',
            name: 'name3',
            discription: '3'
          },
          {
            src: require('~/assets/img/4.svg'),
            // 白色
            backgroundColor: '#ffffff',
            name: 'name4',
            discription: '4'
          },
          {
            src: require('~/assets/img/4.svg'),
            // 白色
            backgroundColor: '#ffffff',
            name: 'name5',
            discription: '5'
          },
          {
            src: require('~/assets/img/4.svg'),
            // 白色
            backgroundColor: '#ffffff',
            name: 'name6',
            discription: '6'
          },
          {
            src: require('~/assets/img/4.svg'),
            // 白色
            backgroundColor: '#ffffff',
            name: 'name7',
            discription: '7'
          },
          {
            src: require('~/assets/img/4.svg'),
            // 白色
            backgroundColor: '#ffffff',
            name: 'name8',
            discription: '8'
          },
          {
            src: require('~/assets/img/4.svg'),
            // 白色
            backgroundColor: '#ffffff',
            name: 'name9',
            discription: '9'
          },
          {
            src: require('~/assets/img/4.svg'),
            // 白色
            backgroundColor: '#ffffff',
            name: 'name10',
            discription: '10'
          },
        ],
      }
    },
  }
</script>