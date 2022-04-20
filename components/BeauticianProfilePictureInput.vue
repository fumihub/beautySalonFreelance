<template>
  <section id="profilePicture">
    <v-container fluid>
      <v-row align="center" justify="center">
        <v-col cols="12">
          <v-row align="center" justify="center">
            <v-col cols="12" md="7">
              <h1 class="font-weight-light display-1 text-center ma-5">プロフィール画像入力</h1>
              <p class="font-weight-bold">※画像を1枚は必ず追加してください</p>
              <!-- 画像ファイル入力 -->
              <v-row align="center" justify="center">
                <v-col class="col-12">
                  <div
                    v-for="(profilePictureInput, i) in profilePictureInputs"
                    :key="i"
                  >
                    <p class="font-weight-bold">{{ `画像${i + 1}` }}</p>
                    <img v-if="urls[i] === null" :src="url(i)">
                    <img v-else :src="urls[i]">
                    <v-file-input
                      v-model="profilePictureInput.image"
                      accept="image/*"
                    />
                    <!-- 画像削除ボタン(画像が一つの場合は表示しない) -->
                    <v-row v-if="profilePictureInputs.length !== 1">
                      <v-col class="col-12" align="start" justify="center">
                        <v-btn color="#E9A96F" class="my-3" dark @click="destroy(i)">画像削除</v-btn>
                      </v-col>
                    </v-row>
                  </div>
                </v-col>
              </v-row>
              <!-- 画像追加ボタン -->
              <v-row>
                <v-col class="col-12" align="end" justify="center">
                  <v-btn color="#E9A96F" class="my-3" dark @click="add()">画像追加</v-btn>
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
        urls:[null],
        profilePictureInputs: [
          {
            image: null,
          }
        ]
      }
    },
    computed:{
      url(){
        return function (idx) {
          if (this.profilePictureInputs[idx].image === null) {
            return "";
          }
          else {
            // 選択した画像のURLを作成
            return URL.createObjectURL(this.profilePictureInputs[idx].image);
          }
        }
      },
    },
    watch: {
      profilePictureInputs: {
        handler() {
          // profilePictureInputの内容が変更されたら、親コンポーネントにprofilePictureInputを送信
          this.$emit('transmitProfilePicture', this.profilePictureInputs);
        },
        deep: true,
      }
    },
    methods: {
      add () {
        let alertOn = false;
        for (let i = 0; i < this.profilePictureInputs.length; i++) {
          // 画像が入力されているかをチェック
          if (this.profilePictureInputs[i].image === null) {
            alert("画像を入力してください。")
            alertOn = true;
            break;
          }
        }
        // 画像が正常に入力されていたら
        if (!alertOn) {
          for (let i = 0; i < this.profilePictureInputs.length; i++) {
            // 画像のURLを一時格納
            this.urls[i] = URL.createObjectURL(this.profilePictureInputs[i].image);
          }
          // 配列の次の要素にnullを追加
          this.urls.push(null);
          const imageTmp = {
            image: null,
          }
          this.profilePictureInputs.push(imageTmp);
        }
      },
      destroy(idx) {
        const urlTmp = [];
        const profilePictureTmp = [];
        let after = false;
        for (let i = 0; i < this.profilePictureInputs.length; i++) {
          // 削除される画像のインデックスの時
          if (i === idx) {
            // afterをtrueに
            after = true;
            // 削除された画像が最後に追加したものでない時
            if (i !== this.profilePictureInputs.length - 1) {
              // profilePictureInputsの次の要素を追加
              profilePictureTmp.push(this.profilePictureInputs[i + 1]);
              urlTmp.push(this.urls[i + 1]);
            }
          }
          // 削除される画像のインデックスより後のインデックスの時
          else if (after) {
            if (this.profilePictureInputs.length > i + 1) {
              profilePictureTmp.push(this.profilePictureInputs[i + 1]);
              urlTmp.push(this.urls[i + 1]);
            }
          }
          // 削除される画像のインデックスより前のインデックスの時
          else {
            profilePictureTmp.push(this.profilePictureInputs[i]);
            urlTmp.push(this.urls[i]);
          }
        }
        // 削除する画像を抜いた配列を代入
        this.profilePictureInputs = profilePictureTmp;
        this.urls = urlTmp;
      }
    }
  }
</script>

<style lang="scss">
#search {
  background-color: #f4f7f5;
}
</style>
