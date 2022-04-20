<template>
  <section id="menu">
    <v-container fluid>
      <v-row align="center" justify="center">
        <v-col cols="12">
          <v-row align="center" justify="center">
            <v-col cols="12" md="7">
              <h1 class="font-weight-light display-1 text-center ma-5">メニュー情報入力</h1>
              <p class="font-weight-bold">※メニューを1枚は必ず追加してください</p>
              <p class="font-weight-bold">*：必須項目</p>
              <div
                v-for="(menuInput, i) in menuInputs"
                :key="i">
                <!-- メニューNo. -->
                <v-row align="center" justify="center">
                  <v-col class="col-12">
                    <p class="font-weight-bold">{{ `メニュー${i + 1}` }}</p>
                  </v-col>
                </v-row>
                <!-- カットメニュー名入力 -->
                <v-row align="center" justify="center">
                  <v-col class="col-12">
                    <p class="font-weight-bold">*カットメニュー名</p>
                    <v-text-field
                      v-model="menuInput.menu"
                      label="カットメニュー名を入力"
                      :rules="rules"
                      hide-details="auto"
                    >
                    </v-text-field>
                  </v-col>
                </v-row>
                <!-- メニュー詳細入力 -->
                <v-row align="center" justify="center">
                  <v-col class="col-12">
                    <p class="font-weight-bold">*メニュー詳細</p>
                    <v-textarea
                      v-model="menuInput.menuDetail"
                      label="メニュー詳細を入力"
                      :rules="rules"
                      hide-details="auto"
                    >
                    </v-textarea>
                  </v-col>
                </v-row>
                <!-- 料金入力 -->
                <v-row align="center" justify="center">
                  <v-col class="col-12">
                    <p class="font-weight-bold">*料金</p>
                    <v-text-field
                      v-model="menuInput.price"
                      label="料金を入力"
                      :rules="priceRules"
                      type="number"
                      hide-details="auto"
                    >
                    </v-text-field>
                  </v-col>
                </v-row>
                <!-- 処置時間入力 -->
                <v-row align="center" justify="center">
                  <v-col class="col-12">
                    <p class="font-weight-bold">*処置時間</p>
                    <v-combobox
                      v-model="menuInput.timeSelect"
                      :items="times"
                    ></v-combobox>
                  </v-col>
                </v-row>
                <!-- メニュー削除ボタン(メニューが一つの場合は表示しない) -->
                <v-row v-if="menuInputs.length !== 1">
                  <v-col class="col-12" align="start" justify="center">
                    <v-btn color="#E9A96F" class="my-3" dark @click="destroy(i)">メニュー削除</v-btn>
                  </v-col>
                </v-row>
              </div>
              <!-- メニュー追加ボタン -->
              <v-row>
                <v-col class="col-12" align="end" justify="center">
                  <v-btn color="#E9A96F" class="my-3" dark @click="add()">メニュー追加</v-btn>
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
        menuInputs: [
          {
            menu: null,
            menuDetail: null,
            price: null,
            timeSelect: null,
          }
        ],
        rules: [
          value => !!value || 'Required.',
        ],
        priceRules: [
          value => !!value || 'Required.',
          value => (value && value.length <= 6) || 'Max ¥100,000'
        ],
        times: [
          "15分",
          "30分",
          "1時間",
          "1時間30分",
          "2時間",
          "2時間30分",
          "3時間",
          "3時間30分",
          "4時間",
          "4時間30分",
          "5時間",
        ],
      }
    },
    watch: {
      menuInputs: {
        handler() {
          // menuInputsの内容が変更されたら、親コンポーネントにmenuInputsを送信
          this.$emit('transmitMenu', this.menuInputs);
        },
        deep: true,
      }
    },
    methods: {
      add () {
        let alertOn = false;
        for (let i = 0; i < this.menuInputs.length; i++) {
          // 処置時間にその他の入力がないか確認
          let timeCheck = false;
          this.times.forEach(element => {
            if (element === this.menuInputs[i].timeSelect) {
              timeCheck = true;
            }
          });
          // すべての項目が埋まっているかチェック
          if (this.menuInputs[i].menu === null || this.menuInputs[i].menu === ""
          || this.menuInputs[i].menuDetail === null || this.menuInputs[i].menuDetail === ""
          || this.menuInputs[i].price === null || this.menuInputs[i].price === ""
          || this.menuInputs[i].timeSelect === null || this.menuInputs[i].timeSelect === "") {
            alert("全ての項目を埋めてください。")
            alertOn = true;
            break;
          }
          // 料金設定が高すぎないかチェック
          else if (this.menuInputs[i].price > 100000) {
            alert("料金が高すぎます。低くしてください。")
            alertOn = true;
            break;
          }
          // 処置時間にその他の入力がある場合
          else if (!timeCheck) {
            alert("処置時間は選択肢から選んでください。")
            alertOn = true;
            break;
          }
        }
        // すべての項目が正常に埋まっていたら
        if (!alertOn) {
          // 次の要素にnullを追加
          const menuTmp = {
            menu: null,
            menuDetail: null,
            price: null,
            timeSelect: null,
          };
          this.menuInputs.push(menuTmp);
        }
      },
      destroy(idx) {
        const menuTmp = [];
        let after = false;
        for (let i = 0; i < this.menuInputs.length; i++) {
          // 削除されるメニューのインデックスの時
          if (i === idx) {
            // afterをtrueに
            after = true;
            // 削除されたメニューが最後に追加したものでない時
            if (i !== this.menuInputs.length - 1) {
              // menuInputsの次の要素を追加
              menuTmp.push(this.menuInputs[i + 1]);
            }
          }
          // 削除されるメニューのインデックスより後のインデックスの時
          else if (after) {
            if (this.menuInputs.length > i + 1) {
              menuTmp.push(this.menuInputs[i + 1]);
            }
          }
          // 削除されるメニューのインデックスより前のインデックスの時
          else {
            menuTmp.push(this.menuInputs[i]);
          }
        }
        // 削除するメニューを抜いた配列を代入
        this.menuInputs = menuTmp;
      }
    }
  }
</script>

<style lang="scss">
#search {
  background-color: #f4f7f5;
}
</style>
