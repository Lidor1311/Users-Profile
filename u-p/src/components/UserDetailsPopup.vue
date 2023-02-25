<template>
    <div class="popup">
      <div class="popup-content">
        <h2>{{ user.userName }}</h2>
        <p><strong>Email:</strong> {{ user.email }}</p>
        <p><strong>Date of Birth:</strong> {{ user.dateOfBirth }}</p>
        <img :src="getUserPhotoUrl(user)" alt="User photo" width="100" />
        <button @click="$emit('close')">Close</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "user-details-popup",
    props: {
      user: {
        type: Object,
        required: true,
      },
    },
    methods: {
      getUserPhotoUrl(user) {
        if (user.image) {
          const img = new Image();
          img.src = user.image;
          user.image = img.src
          return user.image
          //return URL.createObjectURL(user.image);
        } else {
          return "";
        }
      },
    },
  };
  </script>
  
  <style>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .popup-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    text-align: center;
  }
  </style>
  