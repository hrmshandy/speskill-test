<template>
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
          <tr v-for="item in items" :key="item.product.code">
            <td>
              <div class="product">
                <div class="product__image">
                  <img :src="item.product.media_url" alt="" />
                </div>
                <div class="product__meta">
                  <div class="product__code">{{ item.product.code }}</div>
                  <div class="product__name">{{ item.product.name }}</div>
                  <div class="product__price">
                    {{ formatPrice(item.product.price) }}
                  </div>
                  <div class="product__stock">
                    {{ item.product.stock }} in stock
                  </div>
                </div>
              </div>
            </td>
            <td>
              <input type="text" v-model="item.quantity" class="product__qty" />
            </td>
            <td>
              {{ formatPrice(item.quantity * item.product.price) }}
            </td>
          </tr>
          <tr class="product__footer">
            <td colspan="2" class="total">TOTAL</td>
            <td>{{ formatPrice(total) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { computed, ref } from "vue";

const items = ref([]);

function formatPrice(price) {
  return price.toLocaleString("id-ID", {
    style: "currency",
    currency: "IDR",
  });
}

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
  return items.value
    .map((i) => i.product.price * i.quantity)
    .reduce((partialSum, a) => partialSum + a, 0);
});
</script>
