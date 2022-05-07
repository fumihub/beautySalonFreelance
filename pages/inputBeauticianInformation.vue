<template>
  <v-app>
    <v-main class="pt-0">
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
          <!-- 詳細情報入力画面 -->
          <div v-if="topMenu === '詳細情報'">
            <beautician-profile-input 
              @transmitProfile = "inputProfile"
            />
          </div>
          <!-- カットメニュー入力画面 -->
          <div v-else-if="topMenu === 'カットメニュー'">
            <beautician-menu-input 
              @transmitMenu = "inputMenu"
            />
          </div>
          <!-- プロフィール画像入力画面 -->
          <div v-else-if="topMenu === 'プロフィール画像'">
            <beautician-profile-picture-input 
              @transmitProfilePicture = "inputProfilePicture"
            />
          </div>
        </v-tab-item>
      </v-tabs-items>
      <div align="center" justify="center">
        <v-btn x-large color="#FAE290" class="my-3" @click="transition">
          確認画面へ
        </v-btn>
      </div>
    </v-main>
    <v-scale-transition>
      <v-btn
        v-show="fab"
        v-scroll="onScroll"
        fab
        dark
        fixed
        bottom
        right
        color="secondary"
        @click="toTop"
      >
        <v-icon>mdi-arrow-up</v-icon>
      </v-btn>
    </v-scale-transition>
  </v-app>
</template>

<script>
import BeauticianMenuInput from '../components/BeauticianMenuInput.vue'
import BeauticianProfilePictureInput from '../components/BeauticianProfilePictureInput.vue'
import BeauticianProfileInput from '../components/BeauticianProfileInput.vue'
export default {
  name: 'InputBeauticianInformation',
  components: { BeauticianProfileInput, BeauticianMenuInput, BeauticianProfilePictureInput },
  data() {
    return {
      fab: null,
      color: '',
      flat: null,
      tab: null,
      topMenus: [
        '詳細情報', 'カットメニュー', 'プロフィール画像',
      ],
      profile:{},
      menus: [],
      profilePictures: [],
    }
  },
  watch: {
    fab(value) {
      if (value) {
        this.color = 'secondary'
        this.flat = false
      } else {
        this.color = 'transparent'
        this.flat = true
      }
    },
  },
  created() {
    if (typeof window === 'undefined') {
      return
    }
    // eslint-disable-next-line nuxt/no-globals-in-created
    const top = window.pageYOffset || 0
    if (top <= 60) {
      this.color = 'transparent'
      this.flat = true
    }
  },
  methods: {
    onScroll(e) {
      if (typeof window === 'undefined') return
      const top = window.pageYOffset || e.target.scrollTop || 0
      this.fab = top > 60
    },
    toTop() {
      this.$vuetify.goTo(0)
    },
    inputProfile(profile) {
      // inputProfileをクリア
      this.profile = {};
      // BeauticianProfileInputから帰ってきた値を保持
      this.profile = Object.assign({}, profile);
    }
    ,
    inputMenu(menus) {
      // menuをクリア
      this.menus = [];
      // BeauticianMenuInputから帰ってきた値を保持
      this.menus = menus;
    }
    ,
    inputProfilePicture(profilePictures) {
      // profilePictureをクリア
      this.profilePictures = [];
      // BeauticianProfilePictureInputから帰ってきた値を保持
      this.profilePictures = profilePictures;
    },
    transition() {
      let error = false;
      // エラー処理(profile)----------
      // profileが空の場合
      if (this.isEmpty(this.profile)) {
        alert("詳細情報を入力してください。");
        error = true;
      }
      // profileが空でない場合
      else {
        // 定休日にその他の入力がないか確認
        let holidayCheck = false;
        for (let i = 0; i < this.profile.holidaySelect.length; i++) {
          if (this.profile.holidaySelect[i] === "月"
          || this.profile.holidaySelect[i] === "火"
          || this.profile.holidaySelect[i] === "水"
          || this.profile.holidaySelect[i] === "木"
          || this.profile.holidaySelect[i] === "金"
          || this.profile.holidaySelect[i] === "土"
          || this.profile.holidaySelect[i] === "日") {
            holidayCheck = true;
          }
          else {
            holidayCheck = false;
            break;
          }
        }
        // 名前が未入力
        if (this.profile.lastName === null
        || this.profile.firstName === null) {
          alert("名前を入力してください。");
          error = true;
        }
        // 電話番号が未入力
        else if (this.profile.phoneNumber === null) {
          alert("電話番号を入力してください。");
          error = true;
        }
        // エリアが未入力
        else if (this.profile.area === null) {
          alert("電話番号を入力してください。");
          error = true;
        }
        // 営業時間が未入力
        else if (this.profile.startTime === null
        || this.profile.endTime === null) {
          alert("営業時間を入力してください。");
          error = true;
        }
        // 営業時間の開始時間が終了時間より遅い、または終了時間が開始時間より早い
        else if (this.profile.startTime > this.profile.endTime) {
          alert("営業時間が正確ではありません。正確な営業時間を入力してください。");
          error = true;
        }
        // 支払い方法が未入力
        else if (this.profile.paymentSelect.length === 0) {
          alert("支払い方法を入力してください。");
          error = true;
        }
        // 設備・サービスが未入力
        else if (this.profile.serviceSelect.length === 0) {
          alert("設備・サービスを入力してください。");
          error = true;
        }
        // 得意メニューが未入力
        else if (this.profile.goodMenuSelect.length === 0) {
          alert("得意メニューを入力してください。");
          error = true;
        }
        // 定休日が未入力
        else if (this.profile.holidaySelect.length === 0) {
          alert("定休日を入力してください。");
          error = true;
        }
        // 定休日に任意の文字が入力されている
        else if (!holidayCheck) {
          alert("定休日は選択肢から選んでください。");
          error = true;
        }
      }
      // ---------------------------
      // エラー処理(menus)-------------
      // menuが空の場合
      if (this.isEmpty(this.menus)) {
        alert("カットメニューを入力してください。")
        error = true;
      }
      // menuが空でない場合
      else {
        // 処置時間にその他の入力がないか確認
        let timeCheck = false;
        for (let i = 0; i < this.menus.length; i++) {
          if (this.menus[i].timeSelect === "15分"
          || this.menus[i].timeSelect === "30分"
          || this.menus[i].timeSelect === "1時間"
          || this.menus[i].timeSelect === "1時間30分"
          || this.menus[i].timeSelect === "2時間"
          || this.menus[i].timeSelect === "2時間30分"
          || this.menus[i].timeSelect === "3時間"
          || this.menus[i].timeSelect === "3時間30分"
          || this.menus[i].timeSelect === "4時間"
          || this.menus[i].timeSelect === "4時間30分"
          || this.menus[i].timeSelect === "5時間"
          || this.menus[i].timeSelect === null) {
            timeCheck = true;
          }
          else {
            timeCheck = false;
            break;
          }
        }
        for (let i = 0; i < this.menus.length; i++) {
          // カットメニュー名が未入力
          if (this.menus[i].menu === null
          || this.menus[i].menu === "") {
            alert("カットメニュー名を入力してください。");
            error = true;
            break;
          }
          // メニュー詳細が未入力
          else if (this.menus[i].menuDetail === null
          || this.menus[i].menuDetail === "") {
            alert("メニュー詳細を入力してください。");
            error = true;
            break;
          }
          // 料金が未入力
          else if (this.menus[i].price === null
          || this.menus[i].price === "") {
            alert("料金を入力してください。");
            error = true;
            break;
          }
          // 処置時間が未入力
          else if (this.menus[i].timeSelect === null
          || this.menus[i].timeSelect === "") {
            alert("処置時間を入力してください。");
            error = true;
            break;
          }
          // 処置時間に任意の文字が入力されている
          else if (!timeCheck) {
            alert("処置時間は選択肢から選んでください。");
            error = true;
            break;
          }
        }
      }
      // ---------------------------
      // ---------------------------
      // エラー処理(profilePicture)---
      // profilePictureが空の場合
      if (this.isEmpty(this.profilePictures)) {
        alert("プロフィール画像を入力してください。")
        error = true;
      }
      // profilePictureが空でない場合
      else {
        for (let i = 0; i < this.profilePictures.length; i++) {
          // 画像が未入力
          if (this.profilePictures[i].image === null) {
            alert("プロフィール画像を入力してください。");
            error = true;
            break;
          }
        }
      }
      // ---------------------------
      // エラーがなければ、確認画面へ(引数：profile, menu, profilePicture)
      if (!error) {
        // 確認画面に遷移する
        this.$router.push({
          path: "/checkBeauticianInformation",
          query: { profile: this.profile, menus: this.menus, profilePictures: this.profilePictures },
        });
      }
    },
    isEmpty(obj){
      // オブジェクトが空かどうか判定
      return !Object.keys(obj).length;
    },
  },
}
</script>

<style lang="scss">
.v-main {
  background-image: url('~@/assets/img/bgMain.png');
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
}
</style>
