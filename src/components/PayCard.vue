<template>
  <div class="card-wrapper rounded-lg overflow-hidden mx-auto rounded-lg">
    <div class="card w-full h-full">
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
      <div class="card-back">
        <div class="card-black-line"></div>
        <div class="name-and-CVC">
          <div class="name">
            <div
              class="name-background"
              :style="{ top: item * 20 - 10 + '%' }"
              v-for="item in 5"
            ></div>
            <input class="name-content w-full" type="text" v-model="owner" />
          </div>
          <div class="CVC">
            <input
              class="CVC-content text-white"
              type="text"
              v-model="CVCnumber"
            />
          </div>
        </div>
        <div class="visa-logo">
          <img src="https://i.imgur.com/lokBLnp.png" alt="" />
        </div>
      </div>
    </div>
  </div>

  <div class="card-info mx-auto grid grid-rows-4 grid-flow-col gap-4">
    <div class="card-info-number">
      <label class="card-info-number-title">Card Number</label>
      <input
        class="card-info-number-content w-full border border-gray-400 rounded-lg"
        type="text"
      />
    </div>
    <div class="card-info-holder">
      <label class="card-info-holder-title">Card Holder</label>
      <input
        class="card-info-holder-content w-full border border-gray-400 rounded-lg"
        type="text"
      />
    </div>
    <div class="expiration-date-and-CVC grid grid-cols-3 gap-x-4">
      <div class="expiration-date-title col-span-2">Expiration Date</div>

      <div class="expiration-date-content col-span-2 flex">
        <select
          class="w-full h-full border border-gray-400 rounded-lg"
          name="expiration-date-month"
          id="expiration-date-month"
        >
          <option selected="selected">Month</option>
          <option :key="item" v-for="item in 12">{{ item }}</option>
        </select>
        <select
          class="w-full h-full border border-gray-400 rounded-lg"
          name="expiration-date-year"
          id="expiration-date-year"
        >
          <option selected="selected">Year</option>
          <option v-for="item in getNumberRange(2020, 2030)">{{ item }}</option>
        </select>
      </div>
      <div class="CVC-content">
        <input
          class="w-full h-full border border-gray-400 rounded-lg"
          type="text"
        />
      </div>
    </div>
    <div class="submit">
      <button class="w-full h-full rounded-lg">Submit</button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, reactive, ref } from "vue";

defineProps({
  msg: String,
});

const owner = reactive("");
const CVCnumber = reactive("CVC");
const getNumberRange = (start, end) => {
  let result = [],
    count = start ?? 0;
  while (count <= end) {
    result.push(count);
    count++;
  }
  console.log(result);
  return result;
};
</script>

<style lang="scss" scoped>
.card-wrapper {
  max-width: 430px;
  width: 100%;
  height: 270px;
  overflow: hidden;
  margin-bottom: -130px;
  .card {
    -webkit-font-smoothing: antialiased;
    box-shadow: 0 20px 60px 0 rgb(14 42 90 / 55%);
    position: sticky;
    transform-style: preserve-3d;
    transition: 0.5s all ease;
    &:hover {
      transform: rotateY(180deg);
    }

    .card-front {
      padding: 25px 15px;
      height: 100%;
      width: 100%;
      background-image: url("https://i.imgur.com/5XHCjPT.jpg");
      background-size: cover;
      position: absolute;
      backface-visibility: hidden;
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
      position: absolute;
      height: 100%;
      width: 100%;
      background-image: url("https://i.imgur.com/5XHCjPT.jpg");
      background-size: cover;
      backface-visibility: hidden;
      transform: rotateY(180deg);
      text-align: right;
      .card-black-line {
        margin-top: 30px;
        background-color: #000;
        height: 100%;
        max-height: 50px;
      }
      .name-and-CVC {
        display: flex;
        align-items: center;
        margin: 10px 20px 0;

        .name {
          width: 55%;
          background-color: #fff;
          background-image: linear-gradient(
            0.25turn,
            rgba(#fff, 0.8),
            rgba(#ffa400, 0.8),
            rgba(#fff, 0.8)
          );
          position: relative;
          .name-background {
            top: 10%;
            position: absolute;
            width: 100%;
            height: 10%;
            background: linear-gradient(
              0.25turn,
              rgba(#fff, 0.8),
              rgba(#3a3f92, 0.8),
              rgba(#fff, 0.8),
              rgba(#3a3f92, 0.8)
            );
            background-size: auto 10%;
            z-index: 1;
          }
          .name-content {
            height: 40px;
            background-color: transparent;
            z-index: 2;
            position: relative;
            padding: 5px 15px;
            text-align: center;
          }
        }
        .CVC {
          .CVC-content {
            width: 2rem;
            margin-left: 5px;
            background-color: transparent;
          }
        }
      }
      .visa-logo {
        height: 45px;
        text-align: right;
        display: flex;
        justify-content: flex-end;
        position: absolute;
        bottom: 20px;
        right: 20px;
        img {
          height: 100%;
        }
      }
    }
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
  .card-info-number {
    .card-info-number-title {
      font-size: 14px;
      font-weight: 500;
      color: #1a3b5d;
    }
    .card-info-number-content {
      height: 50px;
      padding: 5px 15px;
    }
  }
  .card-info-holder {
    .card-info-holder-title {
      font-size: 14px;
      font-weight: 500;
      color: #1a3b5d;
    }
    .card-info-holder-content {
      height: 50px;
      padding: 5px 15px;
    }
  }
  .expiration-date-and-CVC {
    .expiration-date-title {
      font-size: 14px;
      font-weight: 500;
      color: #1a3b5d;
      justify-items: end;
    }
    .CVC-title {
      font-size: 14px;
      font-weight: 500;
      color: #1a3b5d;
      justify-items: end;
    }
    .expiration-date-content {
      height: 50px;
      #expiration-date-month {
        margin-right: 8px;
        padding: 5px 15px;
      }
      #expiration-date-year {
        padding: 5px 15px;
      }
    }
    .CVC-content {
      height: 50px;
      input {
        padding: 5px 15px;
      }
    }
  }
  .submit {
    height: 55px;
    margin: 1em 0;
    button {
      background-color: #2364d2;
      box-shadow: 3px 10px 20px 0 rgb(35 100 210 / 30%);
      color: #fff;
      font-size: 22px;
    }
  }
}
</style>
