<template>
  <div class="app-root" v-if="!appLoading">
    <router-view></router-view>
    <!-- global snackbar -->
    <v-snackbar :timeout="3000" bottom right :color="snackbar.color" v-model="snackbar.show">
      {{ snackbar.text }}
      <v-btn dark flat @click.native="snackbar.show = false" icon>
        <v-icon>close</v-icon>
      </v-btn>
    </v-snackbar>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'

export default {
  data() {
    return {
      rightDrawer: false,
      snackbar: {
        show: false,
        text: "",
        color: ""
      },
      appLoading: true
    }
  },
  created() {
    // add app events
  },
  methods: {
    ...mapActions('auth', {getMe: "getMe"}),
    openThemeSettings() {
      this.$vuetify.goTo(0)
      this.rightDrawer = !this.rightDrawer
    }
  },
  async created () {
    try {
      await this.getMe();
      this.appLoading = false;
    } catch (err) {
      // lgoout
    }
  },
  mounted () {
    
  }
}
</script>
