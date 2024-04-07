<template>
  <view
    class="circle-container"
    :style="boxSize"
  >
    <view
      class="circle"
      :style="circleStyle"
    >
      <view
        v-if="fillet"
        class="border-start"
        :style="borderStyle"
      ></view>
      <view
        class="circle-left ab"
        :style="renderLeftRate(dispalyRate)"
        ><view
          v-if="fillet && dispalyRate >= 50"
          class="border-left-end"
          :style="borderStyle"
        ></view
      ></view>
      <view
        class="circle-right ab"
        :style="renderRightRate(dispalyRate)"
        ><view
          v-if="fillet && dispalyRate < 50"
          class="border-right-end"
          :style="borderStyle"
        ></view
      ></view>
    </view>
    <view
      class="text-area"
      :style="textareaStyle"
      ><slot></slot
    ></view>
  </view>
</template>
<script setup>
import { computed, getCurrentInstance, onMounted, ref } from "vue";

const props = defineProps({
  rate: {
    type: Number,
    require: true,
  },
  boxWidth: {
    type: String,
    default: "200rpx",
  },
  width: {
    type: String,
    default: "20rpx",
  },
  activeColor: {
    type: String,
    default: "#54c4fd",
  },
  inactiveColor: {
    type: String,
    default: "#546063",
  },
  startAngle: {
    type: Number,
    default: 0,
  },
  fillet: {
    type: Boolean,
    default: false,
  },
});

const dispalyRate = computed(() => {
  if (props.rate <= 0) {
    return 0;
  } else if (props.rate >= 100) {
    return 100;
  } else if (props.rate <= 3) {
    return 1;
  } else {
    return props.rate - 3;
  }
});
const circleStyle = computed(() => {
  const width = getNumberAndUnit(props.width).num;
  const unit = getNumberAndUnit(props.width).unit;
  console.log(`box-shadow: inset 0 0 0 ${width + unit} ${props.activeColor};transform:rotate(${props.startAngle}deg)`);
  return `box-shadow: inset 0 0 0 ${width + unit} ${props.activeColor};transform:rotate(${props.startAngle}deg)`;
});
const borderStyle = computed(() => {
  if (dispalyRate.value == 0) return "";
  return `width:${props.width};height:${props.width};background-color:${props.activeColor};`;
});
const textareaStyle = computed(() => {
  return `width:calc(100% - ${props.width});height:calc(100% - ${props.width});`;
});

onMounted(() => {
  // getSize();
});

const renderRightRate = rate => {
  const border = `border: ${props.width} solid ${props.inactiveColor};`;
  if (rate < 50) {
    return border + "transform: rotate(" + 3.6 * rate + "deg);";
  } else {
    return border + `transform: rotate(0);border-color: ${props.activeColor};`;
  }
};

const renderLeftRate = rate => {
  const border = `border: ${props.width} solid ${props.inactiveColor};`;
  if (rate >= 50) {
    return border + "transform: rotate(" + 3.6 * (rate - 50) + "deg);";
  } else {
    return border;
  }
};

const boxSize = ref(`width:${props.boxWidth};height:${props.boxWidth};`);

function getSize() {
  getWidth().then(res => {
    const { width, height } = res;
    const size = width < height ? width : height;
    boxSize.value = `width:${size}px;height:${size}px;`;
  });
}
function getWidth() {
  return new Promise((resolve, reject) => {
    try {
      const { ctx } = getCurrentInstance();
      uni
        .createSelectorQuery()
        .in(ctx)
        .select(".circle-container")
        .boundingClientRect(res => {
          resolve(res);
        })
        .exec();
    } catch (e) {
      //TODO handle the exception
      reject(e);
    }
  });
}

function getNumberAndUnit(str) {
  const numReg = /\d+/g;
  const unitReg = /[a-z]+/;
  const num = str.match(numReg);
  const unit = str.match(unitReg);
  return {
    num: num[0],
    unit: unit[0],
  };
}
</script>

<style lang="scss" scoped>
.circle-container {
  position: relative;
  width: 100%;
  height: 100%;
  .circle {
    position: relative;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    .ab {
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      box-sizing: border-box;
    }
    .circle-left {
      border-radius: 50%;
      clip-path: polygon(0% 0%, 50% 0%, 50% 100%, 0% 100%);
    }

    .circle-right {
      border-radius: 50%;
      clip-path: polygon(50% 0%, 100% 0%, 100% 100%, 50% 100%);
    }
    .border-start {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      z-index: 1;
      border-radius: 50%;
    }
    .border-left-end {
      position: absolute;
      bottom: 0px;
      left: 50%;
      transform: translate(-50%, 100%);
      z-index: 1;
      border-radius: 50%;
    }
    .border-right-end {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translate(-50%, -100%);
      z-index: 1;
      border-radius: 50%;
    }
  }
  .text-area {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 20px;
    height: 20px;
    border-radius: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}
</style>
