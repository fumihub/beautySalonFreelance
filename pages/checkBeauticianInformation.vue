<template>
  <v-app>
    <v-main class="pt-0">
      <carousel-picture-section
        :images="profilePictures"
      />
      <hairdresser-detail-section
        :lastName="profile.lastName"
        :firstName="profile.firstName"
        :phoneNumber="profile.phoneNumber"
        :area="profile.area"
        :startTime="profile.startTime"
        :endTime="profile.endTime"
        :payments="profile.paymentSelect"
        :services="profile.serviceSelect"
        :goodMenus="profile.goodMenuSelect"
        :holidays="profile.holidaySelect"
      />
      <slide-menu-section
        :lastName="profile.lastName"
        :firstName="profile.firstName"
        :menus="menus"
        :introduction="profile.introduction"
        :snsURL="profile.snsURL"
      />
      <div align="center" justify="center">
        <v-btn x-large color="#FAE290" class="my-3" @click="done">
          完了
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
import HairdresserDetailSection from '../components/HairdresserDetailSection.vue'
export default {
  components: { HairdresserDetailSection },
  name: 'HomePage',
  data: () => ({
    fab: null,
    color: '',
    flat: null,
    profile: {},
    menus: [],
    profilePictures: [],
  }),
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

    // queryStringsから受け取った値をdataへ
    this.profile = this.$route.query.profile;
    this.menus = this.$route.query.menus;
    this.profilePictures = this.$route.query.profilePictures;
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
    done() {
      // TODO DBに美容師情報を登録
      // TODO 遷移後戻るボタンを押されるとデータがないためエラーとなるバグ修正
      // ホーム画面に遷移する
      this.$router.push({
        path: "/",
      });
    },
  },
}
</script>

<style scoped>
.v-main {
  background-image: url('~@/assets/img/bgMain.png');
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
}
</style>
