<template class="test">
  <form @submit.prevent="submitForm">
    <label>Username:</label>
    <input type="text" v-model="username" />
    <br />
    <label>Email:</label>
    <input type="email" v-model="email" />
    <br />
    <label>Date of Birth:</label>
    <input type="date" v-model="dateOfBirth" />
    <br />
    <label>Photo:</label>
    <input type="file" @change="handleFileUpload" />
    <br />
    <button type="submit">Submit</button>
  </form>
</template>
<style scoped>
.test {
  width: 80%;
    left: 8%;
    position: absolute !important;
}
.user-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}

.form-group {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
 width: 100%;
}

label {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 10px;
  font-weight: bold;
  height: 50px; 
  right: 25%;
    position: relative;
}

input {
  padding: 9px;
    border: 3px solid #ccc;
    border-radius: 5px;
    width: 50%;
}

button[type="submit"] {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  left: 22%;
  position: absolute;
}

button[type="submit"]:hover {
  background-color: #3e8e41;
}

</style>

<script>
import axios from "axios";

export default {
  name: "UserForm",
  data() {
    return {
      username: "",
      email: "",
      dateOfBirth: "",
      image: "" || null,
    };
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = () => {
        this.image = reader.result;
      };
    },
    submitForm() {
      const formData = new FormData();
      formData.append("username", this.username);
      formData.append("email", this.email);
      formData.append("dob", this.dateOfBirth);
      formData.append("image", this.image);

      const newUser = {
        userName: this.username,
        email: this.email,
        dateOfBirth: this.dateOfBirth,
        image: this.image,
      };
      console.log(newUser.dateOfBirth);
      this.$emit("add-user", newUser);
        console.log(newUser);
      axios
        .post("https://localhost:44354/api/userprofile", newUser)
        .then((response) => {
          console.log(response.data);
          alert("Form submitted successfully!")
        })
        .catch((error) => {
          console.log(error);
          alert('Error submitting form: ' + error)
        });
        
    },
  },
};
</script>
