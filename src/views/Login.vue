<template>
  <v-app class="split-bg">
    <v-content>
      <v-container fluid fill-height>
        <v-layout align-center justify-center class="login-layout">
          <v-flex xs12 sm8 md4 lg4>
            <v-card class="elevation-1 pa-3">
              <v-card-text>
                <div class="layout column align-center">
                  <img
                    src="/img/icons/msapplication-icon-144x144.png"
                    class="auth__logo img-responsive"
                    alt="Vue Material Admin"
                  />
                  <h1 class="flex my-4 primary--text">Sign In</h1>
                </div>
                <v-form>
                  <v-text-field
                    append-icon="person"
                    name="login"
                    label="Login"
                    type="text"
                    v-model="model.UserName"
                  ></v-text-field>
                  <v-text-field
                    append-icon="lock"
                    name="password"
                    label="Password"
                    id="password"
                    type="password"
                    v-model="model.Password"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <div class="login-btn">
                <!--v-btn icon>
                  <v-icon color="blue">fa fa-facebook-square fa-lg</v-icon>
                </v-btn>
                <v-btn icon>
                  <v-icon color="red">fa fa-google fa-lg</v-icon>
                </v-btn>
                <v-btn icon>
                  <v-icon color="light-blue">fa fa-twitter fa-lg</v-icon>
                </v-btn-->
                <v-spacer></v-spacer>
                <v-btn block color="primary" @click="login" :loading="loading">Login</v-btn>
              </div>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { mapActions, mapGetters } from "vuex";

export default {
  data: () => ({
    loading: false,
    model: {
      UserName: "normal@nus.com",
      Password: "N123456"
    }
  }),

  computed: {
    ...mapGetters("auth", {
      // login: "login",
      // dataWeight: "getDataWeight"
    })
  },
  methods: {
    ...mapActions("auth", {
      signin: "login",
      mockTestAPI: "mockTestAPI"
    }),

    login() {
      this.loading = true;
      const formData = new FormData();
      formData.append("Username", this.model.UserName);
      formData.append("Password", this.model.Password);
      this.signin(formData)
        .then(res => {
          if (res.status)
            this.$router.push("/");
          else {
            this.$toast.error('Incorrect username or password');
            this.loading = false;
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  mounted: function() {
    //for api test
    // const formData = new FormData();
    // formData.append("Username", "user06@nus.app");
    // formData.append("Password", "N123456");
    // this.signin(formData)
    //   .then(res => {
    //     //this.$router.push("/");
    //     this.mockTestAPI();
    //   })
    //   .catch(err => {
    //     console.log(err);
    //   });
  }
};
</script>
<style scoped lang="css">
.split-bg {
  height: 50%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  content: "";
  z-index: 0;
}

@media (max-width: 600px) {
  .login-layout {
    align-items: initial;
  }
}
</style>
