<template>
  <div class="container mt-5">
    <div class="card border-0">
      <div class="card-header text-center">เพิ่มโพสต์ใหม่</div>
      <div class="card-body">
        <div class="row g-3">
          <div class="col-md-12">
            <div class="form-group">
              <label for="isIncome">โพสต์</label>
              <textarea v-model="content" cols="30" rows="10" class="form-control"></textarea>
            </div>

            <button
              class="btn mt-3 btn-primary w-100"
              type="button"
              @click="saveData"
              :disabled="isBtnLoad"
            >
              {{ btnMessage }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { addDoc, collection, getFirestore } from "@firebase/firestore";
import { initializeApp } from "@firebase/app";
import { useRouter } from "vue-router";

const content = ref("");

const db = getFirestore(initializeApp);
const router = useRouter();

const [isBtnLoad, btnMessage] = [ref(false), ref("โพสต์")];

async function saveData() {
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโพสต์...."];
  const docRef = {
    content: content.value
  };
  return addDoc(collection(db, "posts"), docRef).then(() => {
    router.push("/");
  }).catch(() => {
      [isBtnLoad.value, btnMessage.value] = [false, "โพสต์"];
      alert("Failed to save");
    });
}
</script>
