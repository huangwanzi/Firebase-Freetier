<template>
  <div class="container">
    <div class="row justify-content-center align-items-center vh-100">
      <div class="col-12 text-center mb-5">
        <img :src="logo" alt="logo" />
      </div>
      <div class="col-md-6">
        <div class="card p-5" style="background-color: #f7f7f7">
          <div class="card-header text-center mb-4"><h2>เข้าสู่ระบบ</h2></div>
          <div class="card-body">
            <div class="alert alert-danger" role="alert" v-if="firebaseError">
              {{ firebaseError }}
            </div>
            <div class="form-group">
              <label for="email">อีเมล</label>
              <input v-model="email" class="form-control" type="text" />
            </div>
            <div class="form-group my-3">
              <label for="password">รหัสผ่าน</label>
              <input v-model="password" class="form-control" type="password" />
            </div>

            <button
              class="btn btn-primary btn-lg rounded-pill d-block mx-auto w-50 mt-4"
              @click="login"
              :disabled="isBtnLoad"
              type="button"
            >
              <span
                v-if="isBtnLoad"
                class="spinner-border spinner-border-sm me-2"
                role="status"
                aria-hidden="true"
              ></span>
              {{ btnMessage }}
            </button>
            <div class="text-center mt-3">
              <span>ยังไม่มีACC?</span>
              <RouterLink
                to="/register"
                class="ms-1 text-primary"
                style="text-decoration: none"
                >สมัครสมาชิก</RouterLink
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
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";
import logo from "@/assets/logo.jpg";

const [email, password] = [ref(""), ref("")];
const firebaseError = ref();
const [isBtnLoad, btnMessage] = [ref(false), ref("เข้าสู่ระบบ")];
const router = useRouter();

function login() {
  const auth = getAuth();
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโหลด...."];
  signInWithEmailAndPassword(auth, email.value, password.value)
    .then(() => {
      router.push("/");
    })
    .catch((error) => {
      const errorMessage = error.message;
      [isBtnLoad.value, btnMessage.value] = [false, "เข้าสู่ระบบ"];
      firebaseError.value = errorMessage;
      // ..
    });
}
</script>
<style scoped></style>
