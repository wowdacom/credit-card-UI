<template>
  <div class="card-wrapper rounded-lg overflow-hidden mx-auto rounded-lg">
    <div
      ref="card"
      :style="{ transform: isBack ? 'rotateY(180deg)' : 'none' }"
      class="card w-full h-full"
    >
      <div class="card-front">
        <div class="card-marker" :style="markerStyle"></div>
        <div class="card-front-content">
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
            <div class="card-holder w-full pr-5">
              <div class="card-holder-title">Card Holder</div>
              <div class="card-holder-content text-white">FULL NAME</div>
            </div>
            <div class="expires">
              <div class="expires-title">Expires</div>
              <div class="expires-content text-white">MM/YY</div>
            </div>
          </div>
        </div>
      </div>
      <div class="card-back">
        <div class="card-marker" :style="markerStyle"></div>
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

  <div
    ref="cardInfo"
    class="card-info mx-auto grid grid-rows-4 grid-flow-col gap-4"
  >
    <div class="card-info-number">
      <label class="card-info-number-title">Card Number</label>
      <input
        class="card-info-number-content w-full border border-gray-400 rounded-lg"
        type="text"
        name="card-info-number"
      />
    </div>
    <div class="card-info-holder">
      <label class="card-info-holder-title">Card Holder</label>
      <input
        class="card-info-holder-content w-full border border-gray-400 rounded-lg"
        type="text"
        name="card-info-holder"
      />
    </div>
    <div class="expiration-date-and-CVC grid grid-cols-3 gap-x-4">
      <div class="expiration-date-title col-span-2">Expiration Date</div>
      <div class="CVC-title">CVC</div>

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
          name="CVC-content"
        />
      </div>
    </div>
    <div class="submit">
      <button class="w-full h-full rounded-lg">Submit</button>
    </div>
  </div>
</template>

<script setup>
import {
  defineProps,
  reactive,
  ref,
  onMounted,
  onUnmounted,
  computed,
} from "vue";

defineProps({
  msg: String,
});

//2/24 要翻面完在抓位置，不然位置會跑掉
//2/24 優化監聽的寫法

const card = ref(null);
const cardInfo = ref(null);
const isAnyFocus = ref(0);
const isBack = ref(false);
const currentSide = ref("front");
const markerCurrentWidth = ref(0);
const markerCurrentHeight = ref(0);
const markerCurrentPositionX = ref(0);
const markerCurrentPositionY = ref(0);

const owner = ref("");
const CVCnumber = ref("CVC");
const getNumberRange = (start, end) => {
  let result = [],
    count = start ?? 0;
  while (count <= end) {
    result.push(count);
    count++;
  }
  return result;
};

onMounted(() => {
  let cardDOM = card.value;
  let cardCodeDOM = cardDOM.querySelector(".card-code");
  let cardHolderDOM = cardDOM.querySelector(".card-holder");
  let cardExpiresDOM = cardDOM.querySelector(".expires");
  let cardCVCDOM = cardDOM.querySelector(".CVC");
  let cardInfoDOM = cardInfo.value;
  let cardNumberDOM = cardInfoDOM.querySelector(
    'input[name="card-info-number"]'
  );
  let cardInfoHolderDOM = cardInfoDOM.querySelector(
    'input[name="card-info-holder"]'
  );

  let expirationDataMonthDOM = cardInfoDOM.querySelector(
    'select[name="expiration-date-month"]'
  );
  let expirationDataYearDOM = cardInfoDOM.querySelector(
    'select[name="expiration-date-year"]'
  );
  let CVCContentDOM = cardInfoDOM.querySelector('input[name="CVC-content"]');

  let changePostion = (cardBox) => {};

  cardNumberDOM.addEventListener("focus", (event) => {
    if (currentSide.value === "front" && isBack.value === false) {
      markerCurrentPositionX.value =
        cardCodeDOM.getBoundingClientRect().left -
        cardDOM.getBoundingClientRect().left;
    } else {
      markerCurrentPositionX.value =
        cardDOM.getBoundingClientRect().width -
        (cardCodeDOM.getBoundingClientRect().left -
          cardDOM.getBoundingClientRect().left) -
        cardCodeDOM.getBoundingClientRect().width;
    }

    markerCurrentPositionY.value =
      cardCodeDOM.getBoundingClientRect().top -
      cardDOM.getBoundingClientRect().top;

    isAnyFocus.value = 1;
    isBack.value = false;
    currentSide.value = "front";

    markerCurrentWidth.value = cardCodeDOM.getBoundingClientRect().width;
    markerCurrentHeight.value = cardCodeDOM.getBoundingClientRect().height;
  });

  cardNumberDOM.addEventListener("blur", (event) => {
    isAnyFocus.value = 0;
  });

  cardInfoHolderDOM.addEventListener("focus", (event) => {
    if (currentSide.value === "front" && isBack.value === false) {
      markerCurrentPositionX.value =
        cardHolderDOM.getBoundingClientRect().left -
        cardDOM.getBoundingClientRect().left;
    } else {
      markerCurrentPositionX.value =
        cardDOM.getBoundingClientRect().width -
        (cardHolderDOM.getBoundingClientRect().left -
          cardDOM.getBoundingClientRect().left) -
        cardHolderDOM.getBoundingClientRect().width;
    }

    markerCurrentPositionY.value =
      cardHolderDOM.getBoundingClientRect().top -
      cardDOM.getBoundingClientRect().top;

    isAnyFocus.value = 1;
    isBack.value = false;
    currentSide.value = "front";

    markerCurrentWidth.value = cardHolderDOM.getBoundingClientRect().width;
    markerCurrentHeight.value = cardHolderDOM.getBoundingClientRect().height;
  });

  cardInfoHolderDOM.addEventListener("blur", (event) => {
    isAnyFocus.value = 0;
  });

  expirationDataMonthDOM.addEventListener("focus", (event) => {
    if (currentSide.value === "front" && isBack.value === false) {
      markerCurrentPositionX.value =
        cardExpiresDOM.getBoundingClientRect().left -
        cardDOM.getBoundingClientRect().left;
    } else {
      markerCurrentPositionX.value =
        cardDOM.getBoundingClientRect().width -
        (cardExpiresDOM.getBoundingClientRect().left -
          cardDOM.getBoundingClientRect().left) -
        cardExpiresDOM.getBoundingClientRect().width;
    }

    markerCurrentPositionY.value =
      cardExpiresDOM.getBoundingClientRect().top -
      cardDOM.getBoundingClientRect().top;

    isAnyFocus.value = 1;
    isBack.value = false;
    currentSide.value = "front";

    markerCurrentWidth.value = cardExpiresDOM.getBoundingClientRect().width;
    markerCurrentHeight.value = cardExpiresDOM.getBoundingClientRect().height;
  });

  expirationDataMonthDOM.addEventListener("blur", (event) => {
    isAnyFocus.value = 0;
  });

  expirationDataYearDOM.addEventListener("focus", (event) => {
    if (currentSide.value === "front" && isBack.value === false) {
      markerCurrentPositionX.value =
        cardExpiresDOM.getBoundingClientRect().left -
        cardDOM.getBoundingClientRect().left;
    } else {
      markerCurrentPositionX.value =
        cardDOM.getBoundingClientRect().width -
        (cardExpiresDOM.getBoundingClientRect().left -
          cardDOM.getBoundingClientRect().left) -
        cardExpiresDOM.getBoundingClientRect().width;
    }

    markerCurrentPositionY.value =
      cardExpiresDOM.getBoundingClientRect().top -
      cardDOM.getBoundingClientRect().top;

    isAnyFocus.value = 1;
    isBack.value = false;
    currentSide.value = "front";

    markerCurrentWidth.value = cardExpiresDOM.getBoundingClientRect().width;
    markerCurrentHeight.value = cardExpiresDOM.getBoundingClientRect().height;
  });

  expirationDataYearDOM.addEventListener("blur", (event) => {
    isAnyFocus.value = 0;
  });

  CVCContentDOM.addEventListener("focus", (event) => {
    if (currentSide.value === "back" && isBack.value === true) {
      markerCurrentPositionX.value =
        cardCVCDOM.getBoundingClientRect().left -
        cardDOM.getBoundingClientRect().left;
    } else {
      markerCurrentPositionX.value =
        cardDOM.getBoundingClientRect().width -
        (cardCVCDOM.getBoundingClientRect().left -
          cardDOM.getBoundingClientRect().left) -
        cardCVCDOM.getBoundingClientRect().width;
    }

    markerCurrentPositionY.value =
      cardCVCDOM.getBoundingClientRect().top -
      cardDOM.getBoundingClientRect().top;

    isAnyFocus.value = 1;
    isBack.value = true;
    currentSide.value = "back";

    markerCurrentWidth.value = cardCVCDOM.getBoundingClientRect().width;
    markerCurrentHeight.value = cardCVCDOM.getBoundingClientRect().height;
  });

  CVCContentDOM.addEventListener("blur", (event) => {
    isAnyFocus.value = 0;
  });
});

const markerStyle = computed(() => {
  return {
    opacity: isAnyFocus.value,
    height: `${markerCurrentHeight.value}px`,
    width: `${markerCurrentWidth.value}px`,
    transform: isBack.value
      ? `translate(${markerCurrentPositionX.value}px,${markerCurrentPositionY.value}px)`
      : `translate(${markerCurrentPositionX.value}px,${markerCurrentPositionY.value}px)`,
  };
});

onUnmounted(() => {
  cardInfo.value
    .querySelector('input[name="card-number"]')
    .removeEventListener("focus", (event) => {
      console.log("hello");
    });
});
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
    transform: rotateY(180deg);
    &:hover {
      transform: rotateY(180deg);
    }

    .card-front {
      height: 100%;
      width: 100%;
      background-image: url("https://i.imgur.com/5XHCjPT.jpg");
      background-size: cover;
      position: absolute;
      backface-visibility: hidden;
      .card-marker {
        position: absolute;
        border: 2px solid hsla(0, 0%, 100%, 0.65);
        border-radius: 5px;
        transition: all 1s;
      }
      .card-front-content {
        padding: 25px 15px;
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
          .card-holder {
            padding: 10px 15px;
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
            padding: 10px 15px;
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
      .card-marker {
        position: absolute;
        border: 2px solid hsla(0, 0%, 100%, 0.65);
        border-radius: 5px;
        transition: all 1s;
      }
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
