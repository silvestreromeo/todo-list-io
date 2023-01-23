<script setup lang="ts">
import { io } from "socket.io-client";

const socket = io("127.0.0.1:3000");
socket.on("connect", () => {
  console.log(`Connection established with id: ${socket.id}`);
});


</script>

<template>
  <div class="menu-container">
    <h1>Join the board!</h1>
  <p>Username: {{ username }}</p>
	<input class="username-input" v-model="username" placeholder="" />
  <br>
  <button class="join-button" @click="$router.push('Board')">Join</button>
</div>
</template>