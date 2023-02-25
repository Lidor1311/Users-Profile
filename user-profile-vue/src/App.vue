<template>
  <div>
    <button @click="showUserForm()">User Form</button>
    <button @click="fetchUsers()">User List</button>
    <hr>
    <div v-if="showUserFormComponent">
      <user-form @add-user="addUser" @close="closePopups"></user-form>
    </div>

    <div v-if="showUserListComponent">
      <user-list :users="users" @close="closePopups" @user-clicked="showUserDetails"></user-list>
    </div>

    <div v-if="showUserDetailsComponent">
      <user-details-popup :user="selectedUser" @close="closePopups"></user-details-popup>
    </div>
  </div>
</template>

<style>
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
button {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 10px; /* optional, adjust as needed */
}

button:hover {
  background-color: #3e8e41;
}

</style>

<script>
import UserFormVue from "./components/UserForm.vue";
import UserListVue from "./components/UserList.vue";
import UserDetailsPopupVue from "./components/UserDetailsPopup.vue";

export default {
  name: "App",
  components: {
    UserForm: UserFormVue,
    UserList: UserListVue,
    UserDetailsPopup : UserDetailsPopupVue,
  },
  data() {
    return {
      showUserFormComponent: false,
      showUserListComponent: false,
      showUserDetailsComponent: false,
      users: [],
      selectedUser: null,
    };
  },
  methods: {
    showUserForm() {
      this.showUserFormComponent = true;
      this.showUserListComponent = false;
      this.showUserDetailsComponent = false;
    },
    fetchUsers() {
      fetch("https://localhost:44354/api/userprofile")
        .then((response) => response.json())
        .then((data) => {
          this.users = data;
          this.showUserFormComponent = false;
          this.showUserListComponent = true;
          this.showUserDetailsComponent = false;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    closePopups() {
      this.showUserFormComponent = false;
      this.showUserListComponent = false;
      this.showUserDetailsComponent = false;
      this.selectedUser = null;
    },
    addUser(newUser) {
      this.users.push(newUser);
    },
    showUserDetails(user) {
      this.selectedUser = user;
      this.showUserFormComponent = false;
      this.showUserListComponent = false;
      this.showUserDetailsComponent = true;
    },
  },
};
</script>