<template>
  <v-toolbar color="primary" fixed dark app>
    <v-toolbar-title>
      <v-toolbar-side-icon @click="handleDrawerToggle"></v-toolbar-side-icon>
    </v-toolbar-title>
    <div v-if="$vuetify.breakpoint.xsOnly && $route.meta.title" class="title-mobile">{{ $route.meta.title }}</div>
    <v-spacer></v-spacer>
    <v-toolbar-items>
      <v-menu offset-y origin="center center" class="elelvation-1" :nudge-bottom="14" transition="scale-transition">
        <v-btn icon flat slot="activator" @click="navigateToFeedback()">
          <v-badge color="red" overlap>
            <span slot="badge">{{getDataUserProfile.unreadReceivedMessages}}</span>
            <v-icon medium>notifications</v-icon>
          </v-badge>
        </v-btn>
        <!--notification-list></notification-list-->
      </v-menu>
      <v-menu offset-y origin="center center" :nudge-bottom="10" transition="scale-transition">
        <v-btn icon large flat slot="activator">
          <v-avatar size="30px">
            <img v-bind:src="getDataUserProfile.avatarURL || '/static/avatar/default.png'" alt="" />
          </v-avatar>
        </v-btn>
        <v-list class="pa-0">
          <v-list-tile
            v-for="(item, index) in items"
            :to="!item.href ? { name: item.name } : null"
            :href="item.href"
            @click="item.click"
            ripple="ripple"
            :disabled="item.disabled"
            :target="item.target"
            rel="noopener"
            :key="index"
          >
            <v-list-tile-action v-if="item.icon">
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>{{ item.title }}</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
      </v-menu>
    </v-toolbar-items>
  </v-toolbar>
</template>
<script>
import NotificationList from "@/components/widgets/list/NotificationList"
import Util from "@/util"
import { mapMutations, mapActions, mapGetters } from "vuex";
export default {
  name: "AppToolbar",
  components: {
    NotificationList
  },
  data() {
    return {
      user: {
        id: "",
        name: "",
        email: "",
        gender: "Male",
        groupName: "Unset",
        measurement: "Metric",
        unreadReceivedMessages: 2,
        avatarURL: '/static/avatar/man_4.jpg'
      },
      items: [
        {
          icon: "account_circle",
          href: "/account",
          title: "Profile",
          click: this.handleProfile
        },
        {
          icon: "fullscreen_exit",
          href: "#",
          title: "Logout",
          click: this.handleLogout
        }
      ]
    }
  },
  computed: {
    ...mapGetters("auth", {
      getDataUserProfile: "getDataUserProfile"
    }),
    toolbarColor() {
      return this.$vuetify.options.extra.mainNav
    }
  },
  methods: {
    ...mapActions("app", {
      resetState: "resetState"
    }),
    ...mapMutations("app", ["setDrawer", "toggleDrawer"]),
    handleDrawerToggle() {
      this.setDrawer(!this.$store.state.app.showDrawer);
    },
    handleFullScreen() {
      Util.toggleFullScreen()
    },
    handleLogout() {
      //handle logout
      localStorage.removeItem('token');
      this.resetState();
      this.$router.push("/auth/login");
    },
    handleSetting() {},
    handleProfile() {},
    navigateToFeedback() {
      this.$router.push(`/coaching_feedback`);
    }
  },
  mounted() {
    /*if (this.getDataUserProfile.length > 0) {
      this.loading = false;
      this.user['avatar'] = this.getDataUserProfile.avatarURL;
      this.user['groupName'] = this.getDataUserProfile.groupName;
      this.user['unreadMessages'] = 2;
      this.displayData();
    }*/
    this.windowWidth = window.innerWidth;
  }
}
</script>
