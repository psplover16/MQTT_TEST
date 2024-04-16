<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank">create-vue</a>, the official Vue + Vite
    starter
  </p>
  <p>
    Install
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>
<script setup>
import { ref, onMounted } from "vue";

defineProps({
  msg: String,
});

const count = ref(0);

import mqtt from "mqtt";

function con() {
  return new Promise((resolve, reject) => {
    try {
      var brokerUrl = "mqtt://mqttgo.io:1883"; // 替换为 mqttgo.io 的地址
      brokerUrl = "wss://broker.mqttgo.io:8084/mqtt"
      var client = mqtt.connect(brokerUrl);
      console.log("ZZ1")
      setTimeout(() => {
        resolve(client);
      }, 2000);
    } catch (err) {
      reject(new Error(err));
    }
  });
}

onMounted(() => {
  con()
    .then((result) => {
      console.log("ZZ2")
      result.subscribe('school/light');
      result.on('message', (topic, message) => {
        console.log(`Received message on topic: ${topic}`);
        console.log(`Message: ${message.toString()}`);
        // 在收到消息後，可以進行相應的處理
      });
      console.log(result);
    })
    .catch((err) => {
      console.log(err);
    });
});
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
