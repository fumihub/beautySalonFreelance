<template>
  <section id="users" class="pb-8">
    <v-container fluid>
      <v-row align="center" justify="center">
        <v-col cols="12">
          <v-card style="width: 100%">
            <v-row class="text-center ma-6">
              <!-- 利用者の方 -->
              <v-col class="col-12 col-sm-6 col-md-4">
                <div class="flex-center">
                  <v-card-text>
                    <div class="flex-center my-3">
                      <img src="~/assets/img/beautySalonUser.png">
                    </div>
                    <h2 class="my-3">利用者の方</h2>
                    <v-btn
                      class="my-3"
                      color="#FAE290"
                      elevation="5">
                      美容師一覧
                    </v-btn>
                  </v-card-text>
                </div>
              </v-col>
              <!-- 美容師の方 -->
              <v-col class="col-12 col-sm-6 col-md-4">
                <div class="flex-center">
                  <v-card-text>
                    <div class="flex-center my-3">
                      <img src="~/assets/img/hairdresser.png">
                    </div>
                    <h2 class="my-3">美容師の方</h2>
                    <v-btn
                      class="my-3"
                        color="#FAE290"
                        elevation="5">
                        新規登録
                      </v-btn>
                  </v-card-text>
                </div>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </section>
</template>

<style lang="scss">
$main_color: #283e79;

</style>

<style scoped>
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

#users {
  z-index: 0;
}

img {
  
  overflow: hidden;
}

section {
  position: relative;
}
</style>

<script>
export default {
  data: () => ({
    
  })
};
</script>