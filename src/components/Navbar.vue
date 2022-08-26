<template>
  <header>
    <nav class="navbar bg-dark navbar-expand-lg">
      <div class="container-fluid">
        <img src="../assets/logo.png" alt="" width="30" height="30" />
        <div class="botones">
          <button class="btn" @click="iniciarConGoogle" v-if="!userGoogle">Acceder</button>
          <button class="btn" @click="salirConGoogle" v-else>Salir</button>
        </div>
      </div>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue";
import { auth } from "../firebase";

import {
  GoogleAuthProvider,
  signInWithPopup,
  onAuthStateChanged,
  signOut,
} from "firebase/auth";

const userGoogle = ref(false);

const iniciarConGoogle = async () => {
  try {
    const provider = new GoogleAuthProvider();
    const { user } = await signInWithPopup(auth, provider);
    //console.log(user);
  } catch (error) {
    console.log(error);
  }
};

const salirConGoogle = async () => {
  await signOut(auth);
};

onAuthStateChanged(auth, (user) => {
  //console.log(user);
  userGoogle.value = user; // es un observador con esto verifico que el usuario este registrado o no
});
</script>

<style>
.btn {
  padding: 0.8em 1.8em;
  border: 2px solid #42b983;
  position: relative;
  overflow: hidden;
  background-color: transparent;
  text-align: center;
  font-size: 16px;
  transition: 0.3s;
  z-index: 1;
  font-family: inherit;
  color: #fff;
}
.btn::before {
  content: "";
  width: 0;
  height: 300%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
  background: #42b983;
  transition: 0.5s ease;
  display: block;
  z-index: -1;
}
.btn:hover::before {
  width: 105%;
}
.btn:hover {
  color: #fff;
}
</style>