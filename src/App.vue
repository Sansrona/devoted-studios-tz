<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div v-for="(key, item) in items" :key="item">{{item}} : {{key}}</div>

  <div class="inputs">
    <label for="text">название контента</label>
    <input type="text" id="text" v-model="key" />
    <label for="content">контент</label>
    <input type="text" id="content" v-model="content" />
    <button @click="submit">Submit</button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
const items = ref([]);
const key = ref('');
const content = ref('');

const submit = async () => {
  const contents = content.value.split(',').map(i => i.trim());
  let resContent = contents.length > 1 ? contents : contents[0];
  let newItem = {
    [key.value]: resContent,
  }
  await fetch('http://localhost:8080/hash', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json;charset=utf-8'
    },
    body: JSON.stringify(newItem)
  }).then(result => result.json()).then(result => items.value = { ...items.value, ...result });
}

onMounted(() => {
  fetch('http://localhost:8080/hash').then(result => result.json()).then(result => items.value = result);
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.inputs {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding-top: 20px;
}
</style>
