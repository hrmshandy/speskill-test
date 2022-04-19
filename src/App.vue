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

  <main class="shop">
    <h3 class="shop__title">SPE Modern Shop</h3>

    <div class="shop-list">
      <table>
        <thead>
          <tr>
            <th>PRODUCT</th>
            <th>QUANTITY</th>
            <th>SUBTOTAL</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items">
            <td>
              <div class="product">
                <div class="product__image">
                  <img :src="item.product.media_url" alt="">
                </div>
                <div class="product__meta">
                  <div class="product__code">{{ item.product.code }}</div>
                  <div class="product__title">{{ item.product.name }}</div>
                  <div class="product__price">{{ item.product.price }}</div>
                  <div class="product__stock">{{ item.product.stock }} in stock</div>
                </div>
              </div>
            </td>
            <td>
              <input type="text" v-model="item.quantity">
            </td>
            <td>
              {{ item.quantity * item.product.price }}
            </td>
          </tr>
          <tr>
            <td colspan="2">TOTAL</td>
            <td>{{ total }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { computed, reactive, ref } from "vue";

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

const items = ref([]);

function fetchItems() {
  axios
    .get("https://spe-academy.spesolution.net/api/recruitment", {
      headers: {
        "Content-Type": "application/json",
        Authorization: "Bearer o7Ytbt9XQLI3PgtebJfKSXKEf0XHU74Y",
      },
    })
    .then(function (response) {
      // handle success
      items.value = response.data;
      console.log(response.data);
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    });
}

fetchItems();

const total = computed(() => {
  return items.value.map(i => i.product.price * i.quantity).reduce((partialSum, a) => partialSum + a, 0)
});
</script>

<style>
@import "./assets/base.css";
</style>
