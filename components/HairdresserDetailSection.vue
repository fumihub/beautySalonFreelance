<template>
    <section id="carouselPicture">
      <v-container fluid>
          <v-row align="center" justify="center">
            <v-col cols="12" sm="10" md="9" lg="7" xl="6">
              <h1 align="center" justify="center" class="font-weight-light display-2">
                  {{ lastName + " " + firstName }}
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

<script>
  
  export default {
    props:{
      lastName: {
        type: String,
        required: true
      },
      firstName: {
        type: String,
        required: true
      },
      phoneNumber: {
        type: String,
        required: true
      },
      area: {
        type: String,
        required: true
      },
      startTime: {
        type: Array,
        required: true
      },
      endTime: {
        type: String,
        required: true
      },
      payments: {
        type: Array,
        required: true
      },
      services: {
        type: Array,
        required: true
      },
      goodMenus: {
        type: Array,
        required: true
      },
      holidays: {
        type: Array,
        required: true
      },
    },
    data () {
      return {
        headers: [
          { text: '項目', value: 'category' },
          { text: '情報', value: 'information' },
        ],
        informations: [],
      }
    },
    created() {
      // 電話番号代入
      const phoneNumber = {
        category: "電話番号",
        information: this.phoneNumber,
      };
      this.informations.push(phoneNumber);
      // エリア代入
      const area = {
        category: "対応可能エリア",
        information: this.area,
      };
      this.informations.push(area);
      // 営業時間代入
      const time = {
        category: "営業時間",
        information: `${this.startTime} ~ ${this.endTime}`,
      };
      this.informations.push(time);
      // 支払い方法代入
      let pay = "";
      for (let i = 0; i < this.payments.length; i++) {
        pay = pay + this.payments[i];
        // 最後の要素以外の場合
        if (i + 1 !== this.payments.length) {
          pay = pay + " / ";
        }
      }
      const payment = {
        category: "支払い方法",
        information: pay,
      };
      this.informations.push(payment);
      // 設備・サービス代入
      let svc = "";
      for (let i = 0; i < this.services.length; i++) {
        svc = svc + this.services[i];
        // 最後の要素以外の場合
        if (i + 1 !== this.services.length) {
          svc = svc + " / ";
        }
      }
      const service = {
        category: "設備・サービス",
        information: svc,
      };
      this.informations.push(service);
      // 得意メニュー代入
      let gm = "";
      for (let i = 0; i < this.goodMenus.length; i++) {
        gm = gm + this.goodMenus[i];
        // 最後の要素以外の場合
        if (i + 1 !== this.goodMenus.length) {
          gm = gm + " / ";
        }
      }
      const goodMenu = {
        category: "得意メニュー",
        information: gm,
      };
      this.informations.push(goodMenu);
      // 定休日代入
      let day = "";
      for (let i = 0; i < this.holidays.length; i++) {
        day = day + this.holidays[i];
        // 最後の要素以外の場合
        if (i + 1 !== this.holidays.length) {
          day = day + " / ";
        }
      }
      const holiday = {
        category: "得意メニュー",
        information: day,
      };
      this.informations.push(holiday);
    }
  }
</script>

<style lang="scss" scoped>


</style>