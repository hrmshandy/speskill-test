<template>
  <header class="awesome-terminal-container">
    <div class="awesome-terminal">
      <h1 class="awesome-terminal__title">&#x3c; SPE / FRONTEND &#x3e;</h1>
      <p class="awesome-terminal__timestamp">
        {{ date + " " + time.h + ":" + time.m + ":" + time.s }}
      </p>
    </div>
    <div class="gradient-background"></div>
  </header>
</template>

<script setup>
import { computed, reactive } from "vue";

const date = computed(() => {
  const options = { year: "numeric", month: "long", day: "numeric" };
  const today = new Date();

  return today.toLocaleDateString("en-US", options);
});

const time = reactive({
  h: "",
  m: "",
  s: "",
});

function startTime() {
  const today = new Date();
  let h = today.getHours();
  let m = today.getMinutes();
  let s = today.getSeconds();
  m = checkTime(m);
  s = checkTime(s);
  time.h = h;
  time.m = m;
  time.s = s;
  setTimeout(startTime, 1000);
}

function checkTime(i) {
  if (i < 10) {
    i = "0" + i;
  } // add zero in front of numbers < 10
  return i;
}

startTime();
</script>
