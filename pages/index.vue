<script setup lang="ts">
const alpha = ref<number | null>(null);
const beta = ref<number | null>(null);
const gamma = ref<number | null>(null);

/**
 * refにイベントで取得した傾きを格納する
 * @param e DeviceOrientationEvent
 */
const handleOrientation = (e: DeviceOrientationEvent) => {
  alpha.value = e.alpha;
  beta.value = e.beta;
  gamma.value = e.gamma;
};

/**
 * gammaの度数を算出する
 */
const gammaDeg = computed(() => {
  if (!gamma.value) return "0deg";
  return `${(gamma.value + 90) * 2}deg`;
});

/**
 * ユーザーの許可ボタン
 */
const requestDeviceOrientationPermission = () => {
  if (
    DeviceOrientationEvent &&
    typeof DeviceOrientationEvent.requestPermission === "function"
  ) {
    DeviceOrientationEvent.requestPermission()
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
        class="hologram w-[300px] h-[300px] flex items-center justify-center bg-[length:30px_30px]"
      >
        <img
          src="https://res.cloudinary.com/ddyzss9j2/image/upload/v1697272733/icon/Rectangle_1_iqgv23.png"
          width="300"
        />
      </div>
    </div>
    <div class="text-center">
      <p>alpha: {{ Math.round(alpha ?? 0) ?? "-" }}</p>
      <p>beta: {{ Math.round(beta ?? 0) ?? "-" }}</p>
      <p>gamma: {{ Math.round(gamma ?? 0) ?? "-" }}</p>=
    </div>
    <div class="flex justify-center">
      <button
        @click="requestDeviceOrientationPermission"
        class="text-base bg-emerald-400 rounded-md px-6 py-3 text-white cursor-pointer"
      >
        傾きを検知する
      </button>
    </div>
  </div>
</template>

<style scoped>
.hologram {
  background-image: repeating-conic-gradient(
    from v-bind(gammaDeg),
    rgb(0, 34, 255) 0%,
    rgb(0, 255, 242) 25%,
    rgb(255, 255, 255) 50%,
    rgb(0, 255, 242) 75%,
    rgb(0, 34, 255) 100%
  );
  filter: drop-shadow(4px 4px 4px rgb(0, 0, 0));
}
</style>
