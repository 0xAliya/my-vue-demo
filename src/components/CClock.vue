<script setup lang="ts">
/**
 * @file A Vue clock component
 * from https://svelte.dev/examples/clock
 */
import { ref, onMounted, onUnmounted, computed } from "vue";
import './CClock.css';
const time = ref(new Date());

const now = computed(() => {
  return {
    hours: time.value.getHours(),
    minutes: time.value.getMinutes(),
    seconds: time.value.getSeconds(),
  };
});

let interval: number;
onMounted(() => {
  interval = setInterval(() => {
    time.value = new Date();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<template>
  <svg viewBox="-50 -50 100 100">
    <circle class="clock-face" r="48" />

    <template
      v-for="minute of [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55]"
      :key="minute"
    >
      <line
        class="major"
        y1="35"
        y2="45"
        :transform="`rotate(${30 * minute})`"
      />
      <line
        v-for="offset of [1, 2, 3, 4]"
        :key="offset"
        class="minor"
        y1="42"
        y2="45"
        :transform="`rotate(${6 * (minute + offset)})`"
      />
    </template>

    <!-- hour hand -->
    <line
      class="hour"
      y1="2"
      y2="-20"
      :transform="`rotate(${30 * now.hours + now.minutes / 2})`"
    />

    <!-- minute hand -->
    <line
      class="minute"
      y1="4"
      y2="-30"
      :transform="`rotate(${6 * now.minutes + now.seconds / 10})`"
    />

    <!-- second hand -->
    <g :transform="`rotate(${6 * now.seconds})`">
      <line class="second" y1="10" y2="-38" />
      <line class="second-counterweight" y1="10" y2="2" />
    </g>
  </svg>
</template>

