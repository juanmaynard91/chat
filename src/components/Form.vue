<template>
  <footer class="navbar navbar-expand-lg bg-dark footer">
    <form @submit.prevent="enviarMensaje">
      <input type="text" class="input" placeholder="Mensaje" v-model.trim="mensaje" />
    </form>
  </footer>
</template>

<script setup>
import { ref } from "vue";
import { db, auth } from "../firebase";
import { addDoc, collection } from "firebase/firestore";
import Swal from "sweetalert2";

const mensaje = ref("");

const enviarMensaje = async () => {
  try {
    if (mensaje.value === "") {
      Swal.fire({
        text: "No puedes ingresar un mensaje vacio!",
        icon: "error",
      });
    } else {
      // currentUser : trae la session activa del usuario (para no andar haciendo props, firebase tiene el currentUser)
      await addDoc(collection(db, "chats"), {
        text: mensaje.value,
        time: new Date().toLocaleTimeString(),
        uid: auth.currentUser.uid,
        displayName: auth.currentUser.displayName,
      });
    }
  } catch (error) {
    console.log(error);
  }
  mensaje.value = "";
};
</script>

<style>
.footer {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 25px;
  width: 100%;
  height: 4rem;
}

form {
  width: 98%;
}

.input {
  width: 100%;
  font-weight: 500;
  font-size: 80%;
  color: #fff;
  background-color: rgb(28, 28, 30);
  box-shadow: 0 0 0.4vw rgba(0, 0, 0, 0.5), 0 0 0 0.15vw transparent;
  border-radius: 0.4vw;
  border: none;
  outline: none;
  padding: 0.4vw;
  transition: 0.4s;
}

.input:hover {
  box-shadow: 0 0 0 0.15vw rgba(135, 207, 235, 0.186);
}

.input:focus {
  box-shadow: 0 0 0 0.15vw #42b983;
}

@media (width: 412px) {
  form {
    width: 95%;
  }
  .input {
    font-size: 90%;
  }
}
</style>