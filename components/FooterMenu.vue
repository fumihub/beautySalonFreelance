<template>
  <v-footer padless>
    <v-card flat tile class="text-center">
      <v-card-text>
        <nav>
          <ul class="footer-link-wraper px-2">
            <li v-for="(item, index) in footerList" :key="index">
              <a class="mx-2 footer-link" :href="item.link" target="_blank">
                {{ item.text }}
              </a>
            </li>
          </ul>
        </nav>
      </v-card-text>
      <v-divider></v-divider>
      <v-card-text>
        <v-row justify="center" no-gutters class="py-3">
          <v-col class="text-center" cols="12">
            <span
              >&copy; {{ new Date().getFullYear() }} 美容師フリーランス</span
            >
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  data: () => ({
    footerList: [
      {
        link: '',
        text: '会社概要',
      },
      {
        link: '',
        text: 'ご利用規約',
      },
      {
        link: '',
        text: 'プライバシーポリシー',
      },
    ],
  }),
}
</script>

<style>
a.footer-link {
  display: inline;
  text-decoration: none;
  color: #3a3a3a;
}
.v-card {
  width: 100%;
}
nav {
  text-align: center;
}

nav ul {
  grid-template-columns: repeat(3, 1fr);
  display: grid;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

nav ul li {
  display: block;
  list-style: none;
}

nav ul li a .footer-link {
  font-size: 8px;
  display: inline-block;
  text-decoration: none;
  color: #333;
}
</style>
