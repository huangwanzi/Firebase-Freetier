<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-12 col-md-3">
        <img :src="logo" alt="logo" />
      </div>
      <div class="col-12 col-md-9">
        <div class="card">
          <div class="card-header text-center"><h2>สมัครแล่</h2></div>
          <div class="card-body">
            <div class="alert alert-danger" role="alert" v-if="firebaseError">
              {{ firebaseError }}
            </div>
            <div class="form-group">
              <label for="email">อีเมล</label>
              <input v-model="email" class="form-control" type="text" />
            </div>
            <div class="form-group">
              <label for="password">รหัสผ่าน</label>
              <input v-model="password" class="form-control" type="password" />
            </div>

            <button
              class="btn btn-success d-block mx-auto"
              @click="register"
              :disabled="isBtnLoad"
              type="button"
            >
              {{ btnMessage }}
            </button>
            <div class="text-center">
              <span class="ms-1">มีACCแล้ว</span>
              <RouterLink to="/" class="ms-1" style="text-decoration: none"
                >เข้าสู่ระบบ</RouterLink
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { RouterLink, useRouter } from "vue-router";
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
import logo from "@/assets/logo.png";

const [email, password] = [ref(""), ref(""), ref("")];
const firebaseError = ref();
const [isBtnLoad, btnMessage] = [ref(false), ref("สมัครสมาชิก")];
const router = useRouter();

function register() {
  const auth = getAuth();
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโหลด...."];
  createUserWithEmailAndPassword(auth, email.value, password.value)
    .then(() => {
      router.push("/");
    })
    .catch((error) => {
      const errorMessage = error.message;
      [isBtnLoad.value, btnMessage.value] = [false, "สมัครสมาชิก"];
      firebaseError.value = errorMessage;
      // ..
    });
}
</script>
