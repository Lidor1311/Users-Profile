<template>
  <table>
    <thead>
      <tr>
        <th>Username</th>
        <th>Email</th>
        <th>Date of Birth</th>
        <th>Photo</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user in users" :key="user.id" @click="showUserDetails(user)">
        <td>{{ user.userName }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.dateOfBirth }}</td>
        <td>
          <img :src="getUserPhotoUrl(user)" alt="User photo" width="50" />
        </td>
      </tr>
    </tbody>
  </table>
  <user-details-popup :user="selectedUser" v-if="selectedUser" @close="closeUserDetails" />
</template>
<style>
table {
  border-collapse: collapse;
  width: 70%;
}

th,
td {
  border: 1px solid rgb(156, 156, 156);
  padding: 8px;
}

th {
  background-color: #5f70bb;
  text-align: left;
}
</style>
<script>
import UserDetailsPopup from './UserDetailsPopup.vue';

export default {
  name: "UserList",
  props: {
    users: {
      type: Array,
      required: true,
    },
  },
  components: {
    UserDetailsPopup,
  },
  data() {
    return {
      selectedUser: null,
    };
  },
  methods: {
    showUserDetails(user) {
      this.selectedUser = user;
    },
    closeUserDetails() {
      this.selectedUser = null;
    },
    getUserPhotoUrl(user) {
      if (user.image) {
        const img = new Image();
        img.src = user.image;
        user.image = img.src;
        return user.image;
        
      } else {
        return "";
      }
    },
  },
};
</script>