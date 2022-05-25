<template>
  <v-app>
    <v-main class="pt-0">
      <v-tabs v-model="tab" grow>
        <v-tabs-slider color="yellow"></v-tabs-slider>
        <v-tab v-for="topMenu in topMenus" :key="topMenu">
          {{ topMenu }}
        </v-tab>
      </v-tabs>
      <v-tabs-items v-model="tab">
        <v-tab-item v-for="topMenu in topMenus" :key="topMenu">
          <!-- 詳細情報入力画面 -->
          <div v-if="topMenu === '詳細情報'">
            <beautician-profile-input @transmitProfile="inputProfile" />
          </div>
          <!-- カットメニュー入力画面 -->
          <div v-else-if="topMenu === 'カットメニュー'">
            <beautician-menu-input @transmitMenu="inputMenu" />
          </div>
          <!-- プロフィール画像入力画面 -->
          <div v-else-if="topMenu === 'プロフィール画像'">
            <beautician-profile-picture-input
              @transmitProfilePicture="inputProfilePicture"
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
  components: {
    BeauticianProfileInput,
    BeauticianMenuInput,
    BeauticianProfilePictureInput,
  },
  data() {
    return {
      fab: null,
      color: '',
      flat: null,
      tab: null,
      topMenus: ['詳細情報', 'カットメニュー', 'プロフィール画像'],
      profile: {},
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
      this.profile = {}
      // BeauticianProfileInputから帰ってきた値を保持
      this.profile = Object.assign({}, profile)
    },
    inputMenu(menus) {
      // menuをクリア
      this.menus = []
      // BeauticianMenuInputから帰ってきた値を保持
      this.menus = menus
    },
    inputProfilePicture(profilePictures) {
      // profilePictureをクリア
      this.profilePictures = []
      // BeauticianProfilePictureInputから帰ってきた値を保持
      this.profilePictures = profilePictures
    },
    transition() {
      const profileErrorMessage = []
      const menuErrorMessage = []
      const profilePictureErrorMessage = []
      let error = false
      // エラー処理(profile)----------
      // profileが空の場合
      if (this.isEmpty(this.profile)) {
        profileErrorMessage.push('プロフィールを入力してください。')
      }
      // profileが空でない場合
      else {
        // 名前が未入力の場合
        if (
          this.profile.lastName === null ||
          this.profile.firstName === null ||
          this.profile.lastName === '' ||
          this.profile.firstName === ''
        ) {
          profileErrorMessage.push('名前を入力してください。')
        }
        // 電話番号が未入力の場合
        if (
          this.profile.phoneNumber === null ||
          this.profile.phoneNumber === ''
        ) {
          profileErrorMessage.push('電話番号を入力してください。')
        }
        // エリアが未入力
        if (this.profile.area === null || this.profile.area === '') {
          profileErrorMessage.push('エリアを入力してください。')
        }
        // 営業時間が未入力
        if (this.profile.startTime === null || this.profile.endTime === null) {
          profileErrorMessage.push('営業時間を入力してください。')
        }
        // 営業時間の開始時間が終了時間より遅い、または終了時間が開始時間より早い
        if (this.profile.startTime > this.profile.endTime) {
          profileErrorMessage.push(
            '営業時間が正確ではありません。正確な営業時間を入力してください。'
          )
        }
        // 支払い方法が未入力
        if (this.profile.paymentSelect.length === 0) {
          profileErrorMessage.push('支払い方法を入力してください。')
        }
        // 設備・サービスが未入力
        if (this.profile.serviceSelect.length === 0) {
          profileErrorMessage.push('設備・サービスを入力してください。')
        }
        // 得意メニューが未入力
        if (this.profile.goodMenuSelect.length === 0) {
          profileErrorMessage.push('得意メニューを入力してください。')
        }
        // 定休日が未入力
        if (this.profile.holidaySelect.length === 0) {
          profileErrorMessage.push('定休日を入力してください。')
        }
        // 定休日が入力されている場合
        else {
          // 定休日にその他の入力がないか確認
          for (let i = 0; i < this.profile.holidaySelect.length; i++) {
            // 定休日に任意の文字が入力されている
            if (
              this.profile.holidaySelect[i] !== '月' &&
              this.profile.holidaySelect[i] !== '火' &&
              this.profile.holidaySelect[i] !== '水' &&
              this.profile.holidaySelect[i] !== '木' &&
              this.profile.holidaySelect[i] !== '金' &&
              this.profile.holidaySelect[i] !== '土' &&
              this.profile.holidaySelect[i] !== '日' &&
              this.profile.holidaySelect[i] !== '祝日' &&
              this.profile.holidaySelect[i] !== 'なし'
            ) {
              profileErrorMessage.push('定休日は選択肢から選んでください。')
              break
            }
          }
        }
        // 自己紹介が未入力の場合
        if (
          this.profile.introduction === null ||
          this.profile.introduction === ''
        ) {
          profileErrorMessage.push('自己紹介を入力してください。')
        }
      }
      // エラー出力
      if (profileErrorMessage.length) {
        alert(profileErrorMessage.join('\n'))
        error = true
      }
      // ---------------------------
      // エラー処理(menus)-------------
      // menusが空の場合
      if (this.isEmpty(this.menus)) {
        menuErrorMessage.push('カットメニューを入力してください。')
      }
      // menusが空でない場合
      else {
        this.menus.forEach((menu, index) => {
          // カットメニュー名が未入力
          if (menu.menu === null || menu.menu === '') {
            menuErrorMessage.push(
              `メニュー${index + 1}のカットメニュー名を入力してください。`
            )
          }
          // メニュー詳細が未入力
          if (menu.menuDetail === null || menu.menuDetail === '') {
            menuErrorMessage.push(
              `メニュー${index + 1}のメニュー詳細を入力してください。`
            )
          }
          // 料金が未入力
          if (menu.price === null || menu.price === '') {
            menuErrorMessage.push(
              `メニュー${index + 1}の料金を入力してください。`
            )
          }
          // 処置時間が未入力
          if (menu.timeSelect === null || menu.timeSelect === '') {
            menuErrorMessage.push(
              `メニュー${index + 1}の処置時間を入力してください。`
            )
          } else {
            // 処置時間に任意の文字が入力されている
            // eslint-disable-next-line no-lonely-if
            if (
              menu.timeSelect !== '15分' &&
              menu.timeSelect !== '30分' &&
              menu.timeSelect !== '1時間' &&
              menu.timeSelect !== '1時間30分' &&
              menu.timeSelect !== '2時間' &&
              menu.timeSelect !== '2時間30分' &&
              menu.timeSelect !== '3時間' &&
              menu.timeSelect !== '3時間30分' &&
              menu.timeSelect !== '4時間' &&
              menu.timeSelect !== '4時間30分' &&
              menu.timeSelect !== '5時間'
            ) {
              menuErrorMessage.push(
                `メニュー${index + 1}の処置時間は選択肢から選んでください。`
              )
            }
          }
        })
      }
      // エラー出力
      if (menuErrorMessage.length) {
        alert(menuErrorMessage.join('\n'))
        error = true
      }
      // ---------------------------
      // ---------------------------
      // エラー処理(profilePictures)---
      // profilePicturesが空の場合
      if (this.isEmpty(this.profilePictures)) {
        profilePictureErrorMessage.push('プロフィール画像を入力してください。')
      }
      // profilePicturesが空でない場合
      else {
        this.profilePictures.forEach((profilePicture, index) => {
          // 画像が未入力
          if (profilePicture.image === null) {
            profilePictureErrorMessage.push(
              `画像${index + 1}のプロフィール画像を入力してください。`
            )
          }
        })
      }
      // エラー出力
      if (profilePictureErrorMessage.length) {
        alert(profilePictureErrorMessage.join('\n'))
        error = true
      }
      // ---------------------------
      // エラーがなければ、確認画面へ(引数：profile, menu, profilePicture)
      if (!error) {
        // 確認画面に遷移する
        this.$router.push({
          path: '/checkBeauticianInformation',
          query: {
            profile: this.profile,
            menus: this.menus,
            profilePictures: this.profilePictures,
          },
        })
      }
    },
    isEmpty(obj) {
      // オブジェクトが空かどうか判定
      return !Object.keys(obj).length
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
