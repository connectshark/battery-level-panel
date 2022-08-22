<template>
<div class="panel">
  <button class="toggle" @click="useCustomLevel = !useCustomLevel">
    <i v-if="useCustomLevel" class='bx active bx-md bxs-toggle-right' ></i>
    <i v-else class='bx bx-md bx-toggle-left'></i>
    <span>Use custom battery level</span>
  </button>
  <div class="title">
    <template v-if="useCustomLevel || isSupported">
      <h1>Battery</h1>
      <p>{{levelPercent}}%</p>
    </template>
    <p v-else>Device is not Supported</p>
  </div>
  <p class="state">
    <template v-if="charging">
      <span>Charging</span>
      <span class="full-battery sparking"><i class='bx bx-sm bxs-battery-charging'></i></span>
    </template>
    <template v-else-if="levelPercent >= 99">
      <span>Full battery</span>
      <span class="full-battery"><i class='bx bx-sm bxs-battery'></i></span>
    </template>
    <template v-else-if="levelPercent <= 20">
      <span>Low battery</span>
      <span class="low-battery sparking"><i class='bx bx-plug'></i></span>
    </template>
    <template v-else></template>
  </p>
  <div class="battery-container">
    <div class="battery">
      <div
        class="liquid"
        :class="{
          full: levelPercent >= 80,
          medium: levelPercent < 80 && levelPercent >= 50,
          few: levelPercent < 50 && levelPercent > 20,
          'almost-nothing': levelPercent <= 20
        }"
        :style="{ height: (levelPercent + 2) + '%' }"
      ></div>
    </div>
  </div>
  <Transition name="slide">
    <div class="level-bar" v-if="useCustomLevel">
      <input type="range" min="0.01" max="1" step="0.01" v-model.number="customLevel">
      <span>{{ levelPercent }}</span>
    </div>
  </Transition>
</div>
</template>
<script>
import { computed, ref } from 'vue'
import { useBattery } from '@vueuse/core'
export default {
  setup() {
    const { level, charging, isSupported } = useBattery()
    const useCustomLevel = ref(false)
    const customLevel = ref(0.5)
    const levelPercent = computed(() => {
      if (useCustomLevel.value) {
        console.log(customLevel.value)
        return parseInt(customLevel.value * 100)
      } else {
        return parseInt(level.value * 100)
      }
    })




    return {
      levelPercent,
      charging,
      useCustomLevel,
      customLevel,
      isSupported
    }
  }
}
</script>

<style lang="scss" scoped>
.panel {
  --gradient-color-red: linear-gradient(90deg, #de250d 15%, #f97d62 100%);
  --gradient-color-orange: linear-gradient(90deg, #de590d 15%, hsl(54, 90%, 45%) 100%);
  --gradient-color-yellow: linear-gradient(90deg, #dec90d 15%, #6cda0b 100%);
  --gradient-color-green: linear-gradient(90deg, #64EB49 15%, #88D437 100%);
  background-color: hsl(0, 0%, 9%);
  width: min(320px, 90%);
  height: min(240px, 90%);
  border-radius: 1rem;
  padding: 2rem;
  box-sizing: border-box;
  display: grid;
  grid-template-columns: auto 100px;
  grid-template-rows: 80% 20%;
  position: relative;
  .toggle{
    position: absolute;
    background-color: inherit;
    bottom: 110%;
    left: 0;
    right: 0;
    margin: 0 auto;
    border-radius: 3rem;
    padding: .2rem 2rem;
    box-sizing: border-box;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    justify-content: space-between;
    border: none;
    color: #fff;
    font-size: 1rem;
    .active {
      color: #64EB49;
    }
  }
  .level-bar {
    position: absolute;
    background-color: inherit;
    top: 110%;
    left: 0;
    right: 0;
    margin: 0 auto;
    border-radius: 3rem;
    padding: .2rem 2rem;
    box-sizing: border-box;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    justify-content: space-between;
    input {
      margin: 0;
      width: 100%;
    }
    span {
      flex-shrink: 0;
      font-size: 1rem;
      width: 50px;
      text-align: right;
    }
  }
  .title {
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    h1 {
      font-size: 1rem;
    }
    p {
      font-size: 2rem;
      font-weight: bold;
    }
  }
  .state {
    display: flex;
    align-items: center;
    .sparking {
      animation: sparking 3s infinite alternate;
    }
    .full-battery {
      --color: #64EB49;
      color: var(--color);
    }
    .low-battery {
      --color: #de590d;
      color: var(--color);
    }
  }
  .battery-container {
    grid-column: 2 / 3;
    grid-row: 1 / 3;
    .battery {
      border-radius: 3rem;
      box-shadow: inset 20px 0 48px hsl(0, 0%, 16%),
        inset -4px 12px 48px hsl(0, 0%, 56%);
      width: 100%;
      height: 100%;
      display: flex;
      flex-flow: column nowrap;
      justify-content: flex-end;
      overflow: hidden;
      .liquid {
        flex-shrink: 0;
        position: relative;
        transition: height .3s linear;
        box-shadow: inset -10px 0 12px hsla(0, 0%, 0%, .1),
          inset 12px 0 12px hsla(0, 0%, 0%, .15);
        &::before {
          content: '';
          position: absolute;
          height: 8px;
          background-image: inherit;
          box-shadow: inset 0 -3px 6px #00000033;
          top: -4px;
          left: 0;
          width: 100%;
          border-radius: 50%;
        }
      }
    }
  }
}

.slide-enter-active,
.slide-leave-active {
  transition: opacity .5s ease,
    transform .5s ease;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateY(-2rem);
  opacity: 0;
}

.almost-nothing {
  background-image: var(--gradient-color-red);
}
.full {
  background-image: var(--gradient-color-green);
}
.medium {
  background-image: var(--gradient-color-yellow);
}
.few {
  background-image: var(--gradient-color-orange);
}

@keyframes sparking {
  0% {
    text-shadow: 0 0 0 var(--color);
  }
  100% {
    text-shadow: 0 0 12px var(--color);
  }
}
</style>