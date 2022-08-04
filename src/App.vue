<script setup lang="ts">
import { ref, Ref } from "vue";
import Card from "./components/Card.vue";
const imgsArr = [
  "helmet.png",
  "potion.png",
  "ring.png",
  "scroll.png",
  "shield.png",
  "sword.png",
];
const showArr = ref(
  [...imgsArr, ...imgsArr].map((i) => ({ back: i, show: false }))
);
const match = ref<Array<number>>([]);
const check = ref<Array<number>>([]);
const timeout = ref<number | null>(null);
const count = ref<number>(0);

function randomShowArr() {
  for (let i = 0, len = showArr.value.length; i < len; i++) {
    const rand = (Math.random() * len) ^ 0;
    [showArr.value[i], showArr.value[rand]] = [
      showArr.value[rand],
      showArr.value[i],
    ];
  }
  showArr.value = showArr.value.map((i) => ({ ...i, show: false }));
  match.value = [];
  check.value = [];
  timeout.value = null;
  count.value = 0;
}

function handleCardShow(index: number) {
  if (!!timeout.value) return;
  if (match.value.includes(index)) return;
  check.value.push(index);
  showArr.value[index].show = true;

  if (check.value.length >= 2) {
    timeout.value = setTimeout(() => {
      const [first, second] = check.value;
      if (showArr.value[first].back === showArr.value[second].back) {
        match.value.push(first);
        match.value.push(second);
      } else {
        showArr.value[first].show = false;
        showArr.value[second].show = false;
      }
      check.value = [];
      timeout.value = null;
      count.value++;
    }, 600);
  }
}
</script>

<template>
  <div class="container">
    <h2>Magic Match</h2>
    <button @click="randomShowArr">New Game</button>
    <div class="container_cards">
      <Card
        v-for="(item, i) in showArr"
        @show="handleCardShow(i)"
        :show="item.show"
        :back="item.back"
        :key="`${item}_${i}`"
      />
    </div>
    <h4>Turns: {{ count }}</h4>
  </div>
</template>

<style scoped lang="scss">
.container {
  width: 100vw;
  min-height: 100vh;
  overflow: hidden;
  background: rgba($color: #22022b, $alpha: 1);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  h2 {
    color: #fff;
    font-size: 32px;
  }
  button {
    margin: 20px 0;
    border: 2px solid #fff;
    color: #fff;
    background-color: transparent;
    font-size: 18px;
    padding: 10px 20px;
    border-radius: 5px;
    outline: none;
    cursor: pointer;
  }
  &_cards {
    display: flex;
    width: 680px;
    flex-wrap: wrap;
  }
}
h4 {
  color: #fff;
  margin: 20px;
}
</style>
