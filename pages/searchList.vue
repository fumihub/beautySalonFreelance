<template>
  <v-app>
    <v-main class="my-6 pt-0">
      <v-container fluid class="search-form">
        <v-row justify="center">
          <v-col md="10">
            <SearchSection />
          </v-col>
        </v-row>
      </v-container>
      <v-container fluid>
        <v-row justify="center">
          <v-col md="10">
            <div id="search-result">
              <li v-for="item in hairdressers" :key="item.id">
                <v-divider></v-divider>
                <SearchListItem class="my-2" :hairdresser="item" />
              </li>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    hairdressers: [
      {
        id: 1,
        name: 'フリーランス太郎',
        rating: 3,
        ratingCount: 49,
        summary: `テストですーーーー!!!!
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        `,
        images: [
          { src: require('~/assets/img/1.svg'), alt: 'image1' },
          { src: require('~/assets/img/2.svg'), alt: 'image1' },
          { src: require('~/assets/img/1.svg'), alt: 'image1' },
          { src: require('~/assets/img/2.svg'), alt: 'image1' },
        ],
        discription:
          'kakak\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka',
      },
      {
        id: 2,
        name: 'フリーランス花子',
        rating: 2.5,
        ratingCount: 23,
        summary: `テストですーーーー!!!!
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        `,
        images: [
          { src: require('~/assets/img/1.svg'), alt: 'image1' },
          { src: require('~/assets/img/2.svg'), alt: 'image1' },
          { src: require('~/assets/img/1.svg'), alt: 'image1' },
          { src: require('~/assets/img/2.svg'), alt: 'image1' },
        ],
        discription:
          'kakak\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka',
      },
      {
        id: 3,
        name: 'フリーランス大阪',
        rating: 4.5,
        ratingCount: 123,
        summary: `テストですーーーー!!!!
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト これはテストテスト
        `,
        images: [
          { src: require('~/assets/img/1.svg'), alt: 'image1' },
          { src: require('~/assets/img/1.svg'), alt: 'image1' },
          { src: require('~/assets/img/1.svg'), alt: 'image1' },
          { src: require('~/assets/img/2.svg'), alt: 'image1' },
        ],
        discription:
          'kakak\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka\nkakaka',
      },
    ],
    flat: null,
  }),
}
</script>

<style lang="scss" scoped>
li {
  list-style-type: none;
}
#search-result {
  padding: 0 10 0 10;
}
</style>
