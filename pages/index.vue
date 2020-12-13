<template>
  <div class="mb-10">
    <div class="grid grid-cols-8 mx-auto p-2 max-w-3xl">
      <Message
        v-for="(msg, index) in chat"
        :key="index"
        :message="msg"
        :left="msg.userId === 0 ? true : false"
        :hasAvatar="
          index === 0
            ? true
            : chat[index - 1].userId === msg.userId
            ? false
            : true
        "
        :avatar="users[msg.userId].avatar"
        :author="users[msg.userId].name"
      />
    </div>
  </div>
</template>

<script>
import chat from "@/assets/chat.json";
import users from "@/assets/users.json";
import Message from "@/components/Message.vue";
export default {
  components: {
    Message
  },
  data() {
    return {
      chat,
      users
    };
  }
};
</script>

<style></style>
