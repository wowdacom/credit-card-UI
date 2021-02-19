<template>
  <div class="card rounded-lg overflow-hidden mx-auto">
    <div class="card-front">
      <div class="card-logos flex justify-between">
        <div class="anti-fake-logo">
          <img src="https://i.imgur.com/7xhP2ZA.png" alt="" />
        </div>
        <div class="visa-logo">
          <img src="https://i.imgur.com/lokBLnp.png" alt="" />
        </div>
      </div>
      <div class="card-code text-white">
        <span class="card-code-number" v-for="item in 20">{{
          item % 5 === 0 ? " " : "#"
        }}</span>
      </div>
      <div class="card-content flex justify-between">
        <div class="card-holder">
          <div class="card-holder-title">Card Holder</div>
          <div class="card-holder-content text-white">FULL NAME</div>
        </div>
        <div class="expires">
          <div class="expires-title">Expires</div>
          <div class="expires-content text-white">MM/YY</div>
        </div>
      </div>
    </div>
    <div class="card-back"></div>
  </div>
  <div class="card-info mx-auto">
    <div class="card-number">
      <div class="card-number-title">Card Number</div>
      <input class="card-number-content" type="text" />
    </div>
    <div class="card-holder">
      <div class="card-holder-title">Card Holder</div>
      <input class="card-holder-content" type="text" />
    </div>
    <div class="expiration-date-and-CVC">
      <div class="expiration-date">
        <div class="expiration-date-month"></div>
        <div class="expiration-date-year"></div>
        Expiration Date
      </div>
      <div class="CVC"></div>
    </div>

    <div class="submit">
      <button>Submit</button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, reactive, computed } from "vue";

defineProps({
  msg: String,
});

const state = reactive({ count: 0 });

const plusOne = computed({
  get: () => count.value + 1,
  set: (val) => {
    count.value = val - 1;
  },
});
</script>

<style lang="scss" scoped>
.card {
  max-width: 430px;
  width: 100%;
  height: 270px;
  border-radius: 15px;
  overflow: hidden;
  background-color: yellow;
  -webkit-font-smoothing: antialiased;
  box-shadow: 0 20px 60px 0 rgb(14 42 90 / 55%);
  margin-bottom: -130px;
  position: sticky;
  .card-front {
    padding: 25px 15px;
    height: 100%;
    background-image: url("https://i.imgur.com/5XHCjPT.jpg");
    background-size: cover;
    .card-logos {
      height: 45px;
      margin-bottom: 40px;
      padding: 0 10px;
      .anti-fake-logo {
        height: 100%;
        img {
          height: inherit;
        }
      }
      .visa-logo {
        height: 100%;
        img {
          height: inherit;
        }
      }
    }
    .card-code {
      margin-bottom: 25px;
      padding: 10px 15px;
      .card-code-number {
        font-size: 28px;
      }
    }
    .card-content {
      text-align: left;
      padding: 10px 15px;
      .card-holder {
        .card-holder-title {
          opacity: 0.7;
          font-size: 13px;
          --tw-text-opacity: 1;
          color: rgba(255, 255, 255, var(--tw-text-opacity));
        }
        .card-holder-content {
          font-size: 18px;
          white-space: nowrap;
          text-transform: uppercase;
        }
      }
      .expires {
        .expires-title {
          opacity: 0.7;
          font-size: 13px;
          --tw-text-opacity: 1;
          color: rgba(255, 255, 255, var(--tw-text-opacity));
        }
        .expires-content {
          font-size: 18px;
          white-space: nowrap;
          text-transform: uppercase;
        }
      }
    }
  }
  .card-back {
  }
}
.card-info {
  background-color: #fff;
  overflow: hidden;
  box-shadow: 0 30px 60px 0 rgb(90 116 148 / 40%);
  border-radius: 10px;
  max-width: 570px;
  width: 100%;
  padding: 180px 35px 35px;
  z-index: 1;
  .card-number {
  }
}
</style>
