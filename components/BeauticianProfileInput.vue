<template>
  <section id="profile">
    <v-container fluid>
      <v-row align="center" justify="center">
        <v-col cols="12">
          <v-row align="center" justify="center">
            <v-col cols="12" md="7">
              <h1 class="font-weight-light display-1 text-center ma-5">詳細情報入力</h1>
              <p class="font-weight-bold">*：必須項目</p>
              <!-- 名前入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-6">
                  <p class="font-weight-bold">*姓</p>
                  <v-text-field
                    v-model="profileInput.lastName"
                    label="姓を入力"
                    :rules="rules"
                    hide-details="auto"
                  >
                  </v-text-field>
                </v-col>
                <v-col class="col-6">
                  <p class="font-weight-bold">*名</p>
                  <v-text-field
                    v-model="profileInput.firstName"
                    label="名を入力"
                    :rules="rules"
                    hide-details="auto"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
              <!-- 電話番号入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*電話番号</p>
                  <v-text-field
                    v-model="profileInput.phoneNumber"
                    label="電話番号を入力"
                    :rules="rules"
                    hide-details="auto"
                    type="number"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
              <!-- エリア入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*エリア</p>
                  <v-text-field
                    v-model="profileInput.area"
                    label="エリアを入力"
                    :rules="rules"
                    hide-details="auto"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
              <!-- 営業時間入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*営業時間</p>
                  <v-menu
                    ref="menu1"
                    v-model="menu1"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    :return-value.sync="profileInput.startTime"
                    transition="scale-transition"
                    offset-y
                    max-width="290px"
                    min-width="290px"
                  >
                    <template #activator="{ on, attrs }">
                      <v-text-field
                        v-model="profileInput.startTime"
                        label="開始時間"
                        prepend-icon="mdi-clock-time-four-outline"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-time-picker
                      v-if="menu1"
                      v-model="profileInput.startTime"
                      full-width
                      @click:minute="$refs.menu1.save(profileInput.startTime)"
                    ></v-time-picker>
                  </v-menu>
                  <v-menu
                    ref="menu2"
                    v-model="menu2"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    :return-value.sync="profileInput.endTime"
                    transition="scale-transition"
                    offset-y
                    max-width="290px"
                    min-width="290px"
                  >
                    <template #activator="{ on, attrs }">
                      <v-text-field
                        v-model="profileInput.endTime"
                        label="終了時間"
                        prepend-icon="mdi-clock-time-four-outline"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-time-picker
                      v-if="menu2"
                      v-model="profileInput.endTime"
                      full-width
                      @click:minute="$refs.menu2.save(profileInput.endTime)"
                    ></v-time-picker>
                  </v-menu>
                </v-col>
              </v-row>
              <!-- 支払い方法入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*支払い方法</p>
                  <v-combobox
                    v-model="profileInput.paymentSelect"
                    label="その他は入力"
                    :items="payments"
                    multiple
                    chips
                    deletable-chips
                    clearable
                  ></v-combobox>
                </v-col>
              </v-row>
              <!-- 設備・サービス入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*設備・サービス</p>
                  <v-combobox
                    v-model="profileInput.serviceSelect"
                    label="その他は入力"
                    :items="services"
                    multiple
                    chips
                    deletable-chips
                    clearable
                  ></v-combobox>
                </v-col>
              </v-row>
              <!-- 得意メニュー入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*得意メニュー</p>
                  <v-combobox
                    v-model="profileInput.goodMenuSelect"
                    label="その他は入力"
                    :items="goodMenus"
                    multiple
                    chips
                    deletable-chips
                    clearable
                  ></v-combobox>
                </v-col>
              </v-row>
              <!-- 定休日入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*定休日</p>
                  <v-combobox
                    v-model="profileInput.holidaySelect"
                    :items="holidays"
                    multiple
                    chips
                    deletable-chips
                    clearable
                  ></v-combobox>
                </v-col>
              </v-row>
              <!-- 自己紹介入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">*自己紹介</p>
                  <v-textarea
                    v-model="profileInput.introduction"
                    counter
                    label="自己紹介を入力"
                    :rules="rules"
                  ></v-textarea>
                </v-col>
              </v-row>
              <!-- SNSのURL入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <p class="font-weight-bold">SNS</p>
                  <v-text-field
                    v-for="(snsInfo, i) in sns"
                    :key="i"
                    v-model="profileInput.snsURL[i]"
                    :label="snsInfo.label"
                    :prepend-icon="snsInfo.icon"
                    hide-details="auto"
                    class="mt-3"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </section>
</template>

<script>
  export default {
    data(){
      return {
        profileInput:{
          lastName: null,
          firstName: null,
          phoneNumber: null,
          area: null,
          startTime: null,
          endTime: null,
          paymentSelect: [],
          serviceSelect: [],
          goodMenuSelect: [],
          holidaySelect: [],
          introduction: null,
          snsURL: [],
        },
        menu1: false,
        menu2: false,
        rules: [
          value => !!value || 'Required.',
        ],
        payments: [
          "現金",
          "VISA",
          "Master",
          "JCB",
          "AMEX",
          "Diners",
          "ID",
          "楽天Edy",
          "交通系",
          "QuickPay",
          "WAON",
          "PayPay",
          "楽天Pay",
          "LINEPay",
        ],
        services: [
          "早期受付可",
          "深夜受付可",
          "年中無休",
          "当日予約歓迎",
          "子連れ歓迎",
          "ペット連れ歓迎",
        ],
        goodMenus: [
          "ヘアカット",
          "カラー",
          "パーマ",
          "デジタルパーマ",
          "縮毛修正",
          "トリートメント",
          "ヘッドスパ",
          "ヘアセット",
        ],
        holidays: [
          "月",
          "火",
          "水",
          "木",
          "金",
          "土",
          "日",
          "祝日",
          "なし",
        ],
        sns: [
          {
            icon:"mdi-facebook",
            label:"FaceBookのURLを入力",
          },
          {
            icon:"mdi-twitter",
            label:"TwitterのURLを入力",
          },
          {
            icon:"mdi-instagram",
            label:"InstagramのURLを入力",
          },
        ],
      }
    },
    watch: {
      profileInput: {
        handler() {
          // profileInputの内容が変更されたら、親コンポーネントにprofileInputを送信
          this.$emit('transmitProfile', this.profileInput);
        },
        deep: true,
      }
    }
  }
</script>

<style lang="scss">
#search {
  background-color: #f4f7f5;
}
</style>
