<template>
    <section id="carouselPicture">
      <v-container fluid>
          <v-row align="center" justify="center">
            <v-col cols="12" sm="10" md="9" lg="7" xl="6">
              <h1 align="center" justify="center" class="font-weight-light display-2">
                  FullName
              </h1>
              <h1 class="font-weight-bold my-3">
                詳細情報
              </h1>
                <v-data-table
                  :headers="headers"
                  :items="informations"
                  hide-default-header
                  hide-default-footer
                  class="elevation-1"
                ></v-data-table>
            </v-col>
          </v-row>
      </v-container>
    </section>
</template>

<style lang="scss" scoped>


</style>

<script>
  
  export default {
    data () {
      return {
        headers: [
          { text: '項目', value: 'category' },
          { text: '情報', value: 'information' },
        ],
        informations: [
          {
            category: '電話番号',
            information: 'XXX-XXXX-XXXX',
          },
          {
            category: '定休日',
            information: '年中無休(年末年始除く)',
          },
          {
            category: '営業時間',
            information: '【平日】10:00～22:00／【土・日・祝】9:00～22:00 【最終受付】カット 【平日】21:00／【土・日・祝】21:00',
          },
          {
            category: '対応可能エリア',
            information: '東京都',
          },
          {
            category: '支払い方法',
            information: 'VISA / MASTER / JCB / AMEX / Diners / QUICPay / Suica / PASMO / iD',
          },
          {
            category: '得意メニュー',
            information: 'ヘアカットヘア / カラー / パーマ / デジタルパーマ / 縮毛矯正トリートメント / ヘッドスパ / ヘアセット / その他(ヘア)',
          },
          {
            category: '設備・サービス',
            information: '早朝受付可 / 深夜受付可 / 年中無休 / 当日予約歓迎 / 子連れ歓迎 / クレジットカード可',
          },
        ],
      }
    },
  }
</script>