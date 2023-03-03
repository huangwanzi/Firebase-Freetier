<script setup>
import { RouterLink, RouterView, useRouter } from "vue-router";
import { ref } from "vue";
import { onAuthStateChanged, getAuth, signOut } from "firebase/auth";
import logo from "@/assets/logo.jpg";

const isLogin = ref(false);
const router = useRouter();
const firebaseAuth = getAuth();
onAuthStateChanged(firebaseAuth, (user) => {
  if (user) {
    isLogin.value = true;
  } else {
    isLogin.value = false;
  }
});
function signOutButton() {
  signOut(firebaseAuth)
    .then(() => {
      alert("ออกจากระบบแล้ว");
      router.push("/login");
    })
    .catch(() => {
      alert("An error happened during the sign-out process");
    });
}
</script>

<template>
  <header>
    <div class="wrapper">
      <nav class="navbar navbar-expand-lg navbar-dark bg-info">
        <div class="container">
          <RouterLink class="navbar-brand" to="/">
            <img :src="logo" style="height: 40px" alt=""></RouterLink
          ><button
            class="navbar-toggler"
            type="button"
            data-toggle="collapse"
            data-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent"
            aria-expanded="false"
            aria-label="Toggle navigation"
          >
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
              <li class="nav-item active">
                <RouterLink class="nav-link text-white" to="/" v-if="isLogin">หน้าแรก</RouterLink>
              </li>
            </ul>
            <ul class="navbar-nav ms-auto">
              <li class="nav-item active">
                <a class="nav-link text-white" @click="signOutButton" style="cursor: pointer" v-if="isLogin"
                  >ออกจากระบบ</a
                >
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped></style>
