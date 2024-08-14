<script setup lang="ts">
import { inject, ref } from "vue";
import { updateUserNameKey } from "../key";

const userName = ref("");
const updateUserName = inject<(name: string) => void>(
  updateUserNameKey,
  () => {}
);

const emits = defineEmits(["submit"]);
const onSubmit = () => {
  emits("submit", userName.value);
  updateUserName(userName.value);
};
</script>

<template>
  <div class="modal-mask">
    <div class="modal-container">
      <div class="modal-header">
        <slot name="header">Settings</slot>
      </div>
      <div class="modal-body">
        <slot name="body">
          <span>Update Your Name</span>
          <input v-model="userName" />
        </slot>
      </div>
      <div class="modal-footer">
        <slot name="footer">
          <button @click="onSubmit">OK</button>
        </slot>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(38, 38, 38, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-container {
  width: 300px;
  height: 400px;
  background-color: white;
  border-radius: 0.5em;
  display: flex;
  flex-direction: column;
  font-size: 1.5em;
  padding: 20px;
}
span {
  font-size: 0.75em;
}
</style>
