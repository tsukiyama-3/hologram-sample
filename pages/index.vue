<script setup lang="ts">
const alpha = ref<number | null>(null);
const beta = ref<number | null>(null);
const gamma = ref<number | null>(null);

const handleOrientation = (e: DeviceOrientationEvent) => {
  alpha.value = e.alpha;
  beta.value = e.beta;
  gamma.value = e.gamma;
};

const gammaDeg = computed(() => {
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
  <div class="space-y-4">
    <div class="flex h-[50vh] justify-center items-center">
      <div
        class="w-[300px] h-[300px] flex items-center justify-center bg-[length:30px_30px] box"
      >
        <img
          src="https://res.cloudinary.com/ddyzss9j2/image/upload/v1697272733/icon/Rectangle_1_iqgv23.png"
          width="300"
        />
      </div>
    </div>
    <div class="text-center">
      <p>alpha: {{ Math.round(alpha!) ?? "-" }}</p>
      <p>beta: {{ Math.round(beta!) ?? "-" }}</p>
      <p>gamma: {{ Math.round(gamma!) ?? "-" }}</p>
    </div>
    <div class="flex justify-center">
      <button
        @click="requestDeviceMotionPermission"
        class="text-base bg-emerald-400 rounded-md px-6 py-3 text-white cursor-pointer"
      >
        傾きを検知する
      </button>
    </div>
  </div>
</template>

<style scoped>
.box {
  background-image: repeating-conic-gradient(
    from v-bind(gammaDeg),
    hsl(232, 100%, 50%) 0%,
    hsl(177, 100%, 50%) 25%,
    hsl(0, 0%, 100%) 50%,
    hsl(177, 100%, 50%) 75%,
    hsl(232, 100%, 50%) 100%
  );
  filter: drop-shadow(4px 4px 4px rgb(0, 0, 0));
}
</style>
