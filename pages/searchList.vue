<template>
  <v-app>
    <v-main class="my-6 pt-0">
      <v-container fluid class="search-form">
        <SearchSection />
      </v-container>
      <v-container fluid>
        <div id="search-result">
          <li v-for="item in result" :key="item.id">
            <v-divider></v-divider>
            <SearchListItem class="my-2" :hairdresser="item" />
          </li>
        </div>
        <v-divider></v-divider>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    result: [
      {
        id: 1,
        name: 'フリーランス太郎',
        summary: 'テストです。',
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
        summary: 'テストですーーーーー',
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
        summary: 'テストですーーーー!!!!',
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
