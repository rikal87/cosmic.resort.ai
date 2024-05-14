<script setup>
import { ref } from 'vue';
import axios from 'axios';
const conv = ref([]);
function q(e) {
  console.info('e.target.value', e.target.value);
  const v = { text: e.target.value, role: 'user', bubble: false };
  setTimeout(() => {
    v.bubble = true;
    console.info('v.value', v.value.bubble);
  }, 250);
  conv.value.push(v);
  axios.get(`http://localhost:4444/${encodeURI(e.target.value)}`).then((res) => {
    console.info('res', res);
    const r = { text: res.data, role: 'system', bubble: false };
    setTimeout(() => {
      r.bubble = true;
      console.info('r.value', r.bubble);
    }, 250);
    conv.value.push(r);
  });
  e.target.value = null;
}
</script>

<template>
  <div id="wrapper">
    <aside></aside>
    <main>
      <div :class="{ card: true, bubble: chat.bubble }" v-for="chat in conv">{{ chat.text }}</div>
    </main>
  </div>
  <div class="search-bar">
    <input type="search" placeholder="여기에 질문을 입력하세요." @search="q($event)" />
  </div>
</template>

<style lang="scss">
html,
body {
  overflow: hidden;
}
#wrapper {
  position: relative;
  display: grid;
  width: 100%;
  height: 100%;
  overflow: hidden;
  grid-template-columns: 250px auto;
  align-items: end;
  aside {
  }
}
main {
  overflow: hidden;
  position: relative;
  display: block;
}
.search-bar {
  position: fixed;
  margin: auto;
  max-width: 800px;
  width: 100%;
  left: 15px;
  right: 15px;
  bottom: 10px;
}
.card {
  margin-bottom: -100px;
  transition-property: margin;
  transition-duration: 1s;
  transition-delay: 1s;
  &.bubble {
    margin-bottom: 0px;
  }
  & > p.left {
    text-align: left;
  }
  text-align: right;
}
input[type='search'] {
  position: relative;
  &:focus {
    will-change: filter;
    transition: filter 300ms;
    filter: drop-shadow(0 0 1em #42b883aa);
  }
  font-size: 20px;
  padding: 10px;
  outline: unset;
  height: 70px;
  border-radius: 14px;
  border-width: 0;
  width: 100%;
}
</style>
