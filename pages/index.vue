<script setup lang="ts">
const absolute = ref(false);
const alpha = ref<number | null>(null);
const beta = ref<number | null>(null);
const gamma = ref<number | null>(null);

const handleOrientation = (e: DeviceOrientationEvent) => {
  absolute.value = e.absolute;
  alpha.value = e.alpha;
  beta.value = e.beta;
  gamma.value = e.gamma;
};

const gammaPercent = computed(() => {
  if (!gamma.value) return "0deg";
  return `${(gamma.value + 90) * 2}deg`;
});

const requestDeviceMotionPermission = () => {
  if (
    DeviceMotionEvent &&
    typeof DeviceMotionEvent.requestPermission === "function"
  ) {
    DeviceMotionEvent.requestPermission()
      .then((permissionState: string) => {
        if (permissionState === "granted") {
          window.addEventListener(
            "deviceorientation",
            (e: DeviceOrientationEvent) => {
              handleOrientation(e);
            }
          );
        }
      })
      .catch(console.error);
  }
};
</script>

<template>
  <div>
    <div class="container">
      <div class="box">
        <img
          src="https://res.cloudinary.com/ddyzss9j2/image/upload/v1697272733/icon/Rectangle_1_iqgv23.png"
          width="300"
        />
      </div>
    </div>
    <div class="device-motion-info">
      <p>absolute: {{ absolute }}</p>
      <p>alpha: {{ alpha }}</p>
      <p>beta: {{ beta }}</p>
      <p>gamma: {{ gamma }}</p>
    </div>
    <div class="btn-container">
      <button @click="requestDeviceMotionPermission" class="permission-btn">
        傾きを検知する
      </button>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.box {
  width: 300px;
  height: 300px;
  background-image: repeating-conic-gradient(
    from v-bind(gammaPercent),
    hsl(232, 100%, 50%) 0%,
    hsl(177, 100%, 50%) 25%,
    hsl(0, 0%, 100%) 50%,
    hsl(177, 100%, 50%) 75%,
    hsl(232, 100%, 50%) 100%
  );
  background-size: 30px 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  filter: drop-shadow(4px 4px 4px rgb(0, 0, 0));
}

.device-motion-info {
  text-align: center;
}

.btn-container {
  margin-top: 2rem;
  display: flex;
  justify-content: center;
}

.permission-btn {
  font-size: 1rem;
  cursor: pointer;
  background-color: hsl(177, 100%, 50%);
  border-radius: 6px;
  border: none;
  padding: 0.75rem 1.5rem;
  filter: drop-shadow(0 1px 0 rgb(0, 0, 0));
}
</style>
