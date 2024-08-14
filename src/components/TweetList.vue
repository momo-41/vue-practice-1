<script setup lang="ts">
import { PropType } from "vue";
import { Tweet } from "../types/Tweet";

const props = defineProps({
  tweets: {
    type: Array as PropType<Tweet[]>,
  },
});
const emits = defineEmits(["click"]);

const onClickTweet = (id: string) => {
  emits("click", id);
};
</script>

<template>
  <div class="tweet-list">
    <ul>
      <li
        v-for="tweet in props.tweets"
        :key="tweet.id"
        @click="() => onClickTweet(tweet.id)"
      >
        {{ tweet.text }}
        <span class="user-name-span"> @{{ tweet.userName }}</span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.tweet-list li {
  padding: 1em;
  border-bottom: 1px solid #ccc;
  font-size: 1.5em;
}
.user-name-span {
  font-size: 0.75em;
  color: #929292;
}
</style>
