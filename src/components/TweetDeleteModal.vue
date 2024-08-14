<script setup lang="ts">
const emits = defineEmits(["submit", "cancel"]);

const onSubmit = () => {
  emits("submit");
  //   updateUserName(userName.value);
};

const onCancel = () => {
  emits("cancel");
};
</script>

<template>
  <div class="modal-mask">
    <div class="modal-container">
      <div class="modal-header">
        <slot name="header">Delete?</slot>
      </div>

      <div class="modal-footer">
        <slot name="footer">
          <button @click="onSubmit">OK</button>
          <button @click="onCancel">Cancel</button>
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
