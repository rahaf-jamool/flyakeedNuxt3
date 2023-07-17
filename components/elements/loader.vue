<template>
  <div>
    <ClientOnly>
      <div class="main-loader-overlay" v-if="data.show">
        <div class="absCenter">
          <div class="loaderPill">
            <div class="loaderPill-anim">
              <div class="loaderPill-anim-bounce">
                <div class="loaderPill-anim-flop">
                  <div class="loaderPill-pill"></div>
                </div>
              </div>
            </div>
            <div class="loaderPill-floor">
              <div class="loaderPill-floor-shadow"></div>
            </div>
          </div>
        </div>
        <div
          :class="`lds-facebook ${$i18n.locale == 'en' ? 'ltr' : 'rtl'}`"
        ></div>
        <div :class="`loader-percent ${$i18n.locale == 'en' ? 'ltr' : 'rtl'}`">
          {{ data.percent + "%" }}
        </div>
      </div>
    </ClientOnly>
  </div>
</template>

<script setup>
const props = defineProps({
  throttle: {
    type: Number,
    default: 200,
  },
  duration: {
    type: Number,
    default: 2000,
  },
  height: {
    type: Number,
    default: 3,
  },
});
// Options & Data
const data = reactive({
  percent: 0,
  show: false,
  canSucceed: true,
});
// Local variables
let _timer = null;
let _throttle = null;
let _cut;

// Functions
const clear = () => {
  _timer && clearInterval(_timer);
  _throttle && clearTimeout(_throttle);
  _timer = null;
};
const start = () => {
  clear();
  data.percent = 0;
  data.canSucceed = true;

  if (props.throttle) {
    _throttle = setTimeout(startTimer, props.throttle);
  } else {
    startTimer();
  }
};
const set = (num) => {
  data.show = true;
  data.canSucceed = true;
  data.percent = Math.min(100, Math.max(0, Math.floor(num)));
};
const increase = (num) => {
  data.percent = Math.min(100, Math.floor(data.percent + num));
};
const decrease = (num) => {
  data.percent = Math.max(0, Math.floor(data.percent - num));
};
const pause = () => clearInterval(_timer);
const resume = () => startTimer();
const finish = () => {
  // isLoaded.value = useImageLouded().value.loaded
  data.percent = 100;
  hide();
};
const hide = () => {
  clear();
  setTimeout(() => {
    data.show = false;
    // isLoaded.value = useImageLouded().value.loaded = false;
    setTimeout(() => {
      data.percent = 0;
    }, 400);
  }, 500);
};
const startTimer = () => {
  data.show = true;
  _cut = 10000 / Math.floor(props.duration);
  _timer = setInterval(() => {
    increase(_cut);
  }, 100);
};

// Hooks
const nuxtApp = useNuxtApp();

nuxtApp.hook("page:start", start);
nuxtApp.hook("page:finish", finish);

// watchEffect(() => {
//   isLoaded.value = useImageLouded().value.loaded
//   console.log('loader check 1', isLoaded.value);
// })
onBeforeUnmount(() => clear);
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Montserrat");

:root {
  --EASE_INOUT_QUAD: cubic-bezier(0.455, 0.03, 0.515, 0.955);
  --EASE_IN_QUAD: cubic-bezier(0.55, 0.085, 0.68, 0.53);
  --EASE_OUT_QUAD: cubic-bezier(0.25, 0.46, 0.45, 0.94);

  --COLOR_UI_PHARMACY: #0970a5;
  --loaderPill_DURATION: 1800ms;
}

.absCenter {
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.loaderPill {
  text-align: center;
}

.loaderPill-anim {
  height: 160px;
}

.loaderPill-anim-bounce {
  -webkit-animation: loaderPillBounce 1800ms linear infinite;
  animation: loaderPillBounce 1800ms linear infinite;
  -webkit-animation: loaderPillBounce var(--loaderPill_DURATION) linear infinite;
  animation: loaderPillBounce var(--loaderPill_DURATION) linear infinite;
}

.loaderPill-anim-flop {
  transform-origin: 50% 50%;
  -webkit-animation: loaderPillFlop 1800ms linear infinite;
  animation: loaderPillFlop 1800ms linear infinite;
  -webkit-animation: loaderPillFlop var(--loaderPill_DURATION) linear infinite;
  animation: loaderPillFlop var(--loaderPill_DURATION) linear infinite;
}

.loaderPill-pill {
  display: inline-block;
  box-sizing: border-box;
  width: 80px;
  height: 30px;
  border-radius: 15px;
  border: 1px solid #237db5;
  border: 1px solid var(--COLOR_UI_PHARMACY);
  background-image: linear-gradient(to right, #237db5 50%, #ffffff 50%);
  background-image: linear-gradient(
    to right,
    var(--COLOR_UI_PHARMACY) 50%,
    #ffffff 50%
  );
}

.loaderPill-floor {
  display: block;
  text-align: center;
}

.loaderPill-floor-shadow {
  display: inline-block;
  width: 70px;
  height: 7px;
  border-radius: 50%;
  background-color: color(#237db5);
  background-color: color(var(--COLOR_UI_PHARMACY));
  transform: translateY(-15px);
  -webkit-animation: loaderPillScale 1800ms linear infinite;
  animation: loaderPillScale 1800ms linear infinite;
  -webkit-animation: loaderPillScale var(--loaderPill_DURATION) linear infinite;
  animation: loaderPillScale var(--loaderPill_DURATION) linear infinite;
}

.loaderPill-text {
  font-weight: bold;
  color: #237db5;
  color: var(--COLOR_UI_PHARMACY);
  text-transform: uppercase;
}

@-webkit-keyframes loaderPillBounce {
  0% {
    transform: translateY(123px);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  25% {
    transform: translateY(40px);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  50% {
    transform: translateY(120px);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  75% {
    transform: translateY(20px);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  100% {
    transform: translateY(120px);
  }
}

@keyframes loaderPillBounce {
  0% {
    transform: translateY(123px);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  25% {
    transform: translateY(40px);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  50% {
    transform: translateY(120px);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  75% {
    transform: translateY(20px);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  100% {
    transform: translateY(120px);
  }
}

@-webkit-keyframes loaderPillFlop {
  0% {
    transform: rotate(0);
  }
  25% {
    transform: rotate(90deg);
  }
  50% {
    transform: rotate(180deg);
  }
  75% {
    transform: rotate(450deg);
  }
  100% {
    transform: rotate(720deg);
  }
}

@keyframes loaderPillFlop {
  0% {
    transform: rotate(0);
  }
  25% {
    transform: rotate(90deg);
  }
  50% {
    transform: rotate(180deg);
  }
  75% {
    transform: rotate(450deg);
  }
  100% {
    transform: rotate(720deg);
  }
}

@-webkit-keyframes loaderPillScale {
  0% {
    transform: translateY(-15px) scale(1, 1);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  25% {
    transform: translateY(-15px) scale(0.7, 1);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  50% {
    transform: translateY(-15px) scale(1, 1);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  75% {
    transform: translateY(-15px) scale(0.6, 1);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  100% {
    transform: translateY(-15px) scale(1, 1);
  }
}

@keyframes loaderPillScale {
  0% {
    transform: translateY(-15px) scale(1, 1);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  25% {
    transform: translateY(-15px) scale(0.7, 1);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  50% {
    transform: translateY(-15px) scale(1, 1);
    -webkit-animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    -webkit-animation-timing-function: var(--EASE_OUT_QUAD);
    animation-timing-function: var(--EASE_OUT_QUAD);
  }
  75% {
    transform: translateY(-15px) scale(0.6, 1);
    -webkit-animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    animation-timing-function: cubic-bezier(0.55, 0.085, 0.68, 0.53);
    -webkit-animation-timing-function: var(--EASE_IN_QUAD);
    animation-timing-function: var(--EASE_IN_QUAD);
  }
  100% {
    transform: translateY(-15px) scale(1, 1);
  }
}
.landing-page {
  width: 100%;
  height: 100vh;
  position: fixed;
  inset: 0;
  z-index: 9;
}

#myVideo {
  position: fixed;
  right: 0;
  bottom: 0;
  object-fit: fill;
  width: 100vw;
  min-width: 100vw;
  min-height: 100vh;
}

.main-loader-overlay {
  position: fixed;
  background: #14aabb;
  z-index: 999;
}

.loader-percent {
  display: inline-block;
  position: relative;
  top: calc(50% - 40px);
  color: #fff;
  &.rtl {
    right: calc(50% - 97px);
  }
  &.ltr {
    left: calc(50% - 97px);
  }
}

.overflow-section {
  position: absolute;
  background: #263038;
  z-index: 2;
}

.main-loader-overlay,
.overflow-section {
  inset: 0;
  -webkit-backdrop-filter: blur(3px);
  backdrop-filter: blur(3px);
}

.main-loader-overlay .lds-facebook,
.overflow-section .lds-facebook {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
  top: calc(50% - 40px);
  &.rtl {
    right: calc(50% - 40px);
  }
  &.ltr {
    left: calc(50% - 40px);
  }
}

.main-loader-overlay .lds-facebook div,
.overflow-section .lds-facebook div {
  display: inline-block;
  position: absolute;
  left: 8px;
  width: 16px;
  background: #fff;
  -webkit-animation: lds-facebook 1.2s cubic-bezier(0, 0.5, 0.5, 1) infinite;
  animation: lds-facebook 1.2s cubic-bezier(0, 0.5, 0.5, 1) infinite;
}

.main-loader-overlay .lds-facebook div:nth-child(1),
.overflow-section .lds-facebook div:nth-child(1) {
  left: 8px;
  -webkit-animation-delay: -0.24s;
  animation-delay: -0.24s;
}

.main-loader-overlay .lds-facebook div:nth-child(2),
.overflow-section .lds-facebook div:nth-child(2) {
  left: 32px;
  -webkit-animation-delay: -0.12s;
  animation-delay: -0.12s;
}

.main-loader-overlay .lds-facebook div:nth-child(3),
.overflow-section .lds-facebook div:nth-child(3) {
  left: 56px;
  -webkit-animation-delay: 0;
  animation-delay: 0;
}

@-webkit-keyframes lds-facebook {
  0% {
    top: 8px;
    height: 64px;
  }

  50%,
  100% {
    top: 24px;
    height: 32px;
  }
}

@keyframes lds-facebook {
  0% {
    top: 8px;
    height: 64px;
  }

  50%,
  100% {
    top: 24px;
    height: 32px;
  }
}
</style>
