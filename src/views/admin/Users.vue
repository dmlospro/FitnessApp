<template>
  <v-container fluid grid-list-xl>
    <v-layout wrap>
      <v-flex xs12 lg5>
        <CreateNewUser @switchTab="activeTab = 2" class="mb-4" />
        <CreateNewGroup @switchTab="activeTab = 1" />
      </v-flex>
      <v-flex xs12 lg7>
        <v-tabs v-model="activeTab">
          <v-tab>Users</v-tab>
          <v-tab>Groups</v-tab>

          <v-tab-item>
            <UserList />
          </v-tab-item>

          <v-tab-item>
            <GroupList />
          </v-tab-item>
        </v-tabs>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import CreateNewUser from "@/components/user/CreateNewUser"
import CreateNewGroup from "@/components/user/CreateNewGroup"
import UserList from "@/components/user/UserList"
import GroupList from "@/components/user/GroupList"

export default {
  name: "Users",
  components: {
    CreateNewUser,
    CreateNewGroup,
    UserList,
    GroupList
  },
  data() {
    return {
      activeTab: null
    }
  }
}
</script>