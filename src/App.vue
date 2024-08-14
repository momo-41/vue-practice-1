<script setup lang="ts">
import { provide, readonly, ref } from "vue";
import TweetForm from "./components/TweetForm.vue";
import TweetList from "./components/TweetList.vue";
import { Tweet } from "./types/Tweet";
import SettingsModal from "./components/SettingsModal.vue";
import { updateUserNameKey, userNameKey } from "./key";
import TweetDeleteModal from "./components/TweetDeleteModal.vue";

const tweets = ref<Tweet[]>([
  { id: "1", text: "Hello vue.3!", userName: "test" },
  { id: "2", text: "Hello vite!", userName: "test2" },
]);

const onSubmitForm = (tweet: string) => {
  tweets.value.push({
    id: String(Math.random()),
    text: tweet,
    userName: userName.value,
  });
}; //string型のtweetを受け取ってそれにランダムなidをつけて、tweets.valueの配列にpushする

const isModalOpen = ref(false); //この()の中がvalueに当たる。これはfalseがvalue
const onClickSettings = () => {
  isModalOpen.value = true;
};

const onSubmitSetting = (userName: string) => {
  console.log(userName);
  isModalOpen.value = false;
};

const userName = ref("");
const updateUserName = (name: string) => {
  userName.value = name;
};
provide(userNameKey, readonly(userName));
provide(updateUserNameKey, updateUserName);

const isShowDeleteModal = ref(false);
//TweetListでclick時に飛んできたidをApp.vueでもisDeletingIDというリアクティブな定数に入れている
const isDeletingID = ref("");
const onClickTweet = (id: string) => {
  isShowDeleteModal.value = true;
  isDeletingID.value = id;
};

const onDelete = () => {
  deleteTweet(isDeletingID.value);
};

const deleteTweet = (id: string) => {
  tweets.value = tweets.value.filter((tweet) => tweet.id !== id);
  isShowDeleteModal.value = false;
};
</script>

<template>
  <div class="container">
    <div class="header">
      <button @click="onClickSettings">Settings</button>
    </div>
    <Teleport to="body">
      <SettingsModal @submit="onSubmitSetting" v-if="isModalOpen" />
    </Teleport>
    <Teleport to="body">
      <TweetDeleteModal
        v-if="isShowDeleteModal"
        @submit="onDelete"
        @cancel="isShowDeleteModal = false"
      />
    </Teleport>
    {{ userName }}
    <TweetForm @submit="onSubmitForm" />
    <TweetList :tweets="tweets" @click="(id) => onClickTweet(id)" />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
}
.header {
  display: flex;
  justify-content: flex-end;
  padding: 1em;
  button {
    border-radius: 0.5em;
    background-color: #81d6ee;
    color: white;
    width: 120px;
    height: 50px;
    font-size: 1em;
  }
}
</style>
