<template>
  <div class="contents">
    <img
      v-if="hasAvatar && left"
      class="mr-6 inline-block h-16 w-16 rounded-full col-start-1 col-end-2 mt-12"
      :src="imagesPath + this.avatar"
    />
    <div
      class="max-w-sm col-span-6 col-start-2 col-end-7 relative"
      :class="[left ? 'mr-auto' : 'ml-auto', marginTop]"
    >
      <div
        class="absolute bottom-0 text-gray-500 text-xs mb-1"
        :class="left ? '-mr-10 right-0' : '-ml-10 left-0'"
      >
        {{ message.time }}
      </div>
      <div v-if="hasAvatar" class="font-semibold">{{ author }}</div>
      <div
        class="rounded-2xl text-lg bg-white"
        :class="[
          left ? '' : 'bg-gray-200 border-transparent',
          message.type === 'image' ? '' : 'border-2 p-4'
        ]"
      >
        <div class="contents" v-if="message.type === 'text'">
          {{ message.content }}
        </div>
        <div class="contents" v-else-if="message.type === 'audio'">
          <AudioPlayer :fileName="message.content" :left="left" />
        </div>
        <div class="contents" v-else-if="message.type === 'image'">
          <img
            class="rounded-2xl"
            :src="
              imagesPath + message.content
            "
            alt=""
          />
        </div>
      </div>
    </div>
    <img
      v-if="hasAvatar && !left"
      class="ml-6 inline-block h-16 w-16 rounded-full col-start-7 col-end-8 mt-12"
      :src="imagesPath + this.avatar"
    />
  </div>
</template>

<script>
import AudioPlayer from "./AudioPlayer";
export default {
  components: {
    AudioPlayer
  },
  props: {
    message: {
      type: Object
    },
    hasAvatar: {
      type: Boolean
    },
    avatar: {
      type: String
    },
    left: {
      type: Boolean,
      default: true
    },
    author: {
      type: String
    }
  },
  data() {
    return {
      globalPath: "/assets/",
    }
  },
  computed: {
    imagesPath() {
      return this.globalPath + "images/";
    },
    audiosPath() {
      return this.globalPath + "audios/";
    },
    marginTop() {
      let space = "mt-2";
      if (this.hasAvatar) {
        space = "mt-6";
      }
      return space;
    }
  }
};
</script>

<style></style>
