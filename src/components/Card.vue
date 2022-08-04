<template>
  <div :class="['card', show ? 'card_turn' : '']" @click="emit('show')">
    <div class="card_front">
      <img src="../imgs/cover.png" />
    </div>
    <div class="card_back"><img :src="`src/imgs/${back}`" alt="" /></div>
  </div>
</template>
<script lang="ts" setup>
import { ref } from "vue";
interface Props {
  back: string;
  show: boolean;
}
interface Emits {
  (event: "show"): void;
}

const { back, show } = defineProps<Props>();
const emit = defineEmits<Emits>();
</script>
<style scoped lang="scss">
.card {
  position: relative;
  box-sizing: border-box;
  width: 150px;
  height: 150px;
  border: 3px solid #fff;
  border-radius: 5px;
  transition: transform 0.5s;
  user-select: none;
  -webkit-user-select: none;
  margin: 10px;

  &_back,
  &_front {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    font-size: 48px;
    transform: rotateY(0deg);
    color: red;
    backface-visibility: hidden;
    transition: all 0.5s;
    & img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
  &_back {
    transform: rotateY(180deg);
  }
}
.card_turn {
  transform: rotateY(180deg);
  .card_front {
    transform: rotateY(180deg);
  }
  .card_back {
    transform: rotateY(0deg);
  }
}
</style>
