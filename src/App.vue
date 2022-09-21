<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup

import { computed, ref } from "vue";

const text = ref("");
const copyToClipboard = () => {
  const el = document.createElement("textarea");
  el.value = processedText.value;
  document.body.appendChild(el);
  el.select();
  document.execCommand("copy");
  document.body.removeChild(el);
};

enum ModeType {
  Encode,
  Decode,
}

const mode = ref(ModeType.Encode);
const processedText = computed(() =>
  mode.value === ModeType.Encode
    ? encodeURIComponent(text.value)
    : decodeURIComponent(text.value)
);

const toggleMode = () => {
  mode.value =
    mode.value === ModeType.Encode ? ModeType.Decode : ModeType.Encode;
  text.value = "";
};
</script>

<template>
  <div class="bg-white border-slate-100 border rounded p-9">
    <h1 v-if="mode === ModeType.Encode" class="text-black mb-9">
      Text ▶ Encoded Text
    </h1>
    <h1 v-else class="text-black mb-9">Encoded Text ▶ Text</h1>
    <div>
      <!-- row -->
      <div>
        <div class="mb-3 flex flex-col">
          <label for="text" class="font-medium text-gray-700 text-2xl">{{
            mode === ModeType.Encode ? "원본" : "인코딩"
          }}</label>
          <input
            type="text"
            name="text"
            id="text"
            class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 border-gray-300 rounded-md text-4xl"
            v-model="text"
          />
        </div>
        <!-- divider -->
        <div class="m-9 flex flex-row justify-center items-center">
          <hr class="flex-grow" />
          <button @click="toggleMode()">전환</button>
          <hr class="flex-grow" />
        </div>

        <div class="mb-3 flex flex-col">
          <label for="encoded" class="font-medium text-gray-700 text-2xl">{{
            mode === ModeType.Encode ? "인코딩" : "원본"
          }}</label>
          <div class="flex">
            <input
              type="text"
              name="encoded"
              id="encoded"
              class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 border-gray-300 rounded-md text-4xl"
              :value="processedText"
              readonly
            />
            <button
              class="ml-2 bg-red-400 text-white rounded-md"
              @click="copyToClipboard"
            >
              복사
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <small>유진쓰</small>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
