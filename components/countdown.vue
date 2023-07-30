<template>
  <div
    grid
    place-items-center
    h-screen
    bg-gradient-to-b
    from-red-500
    to-blue-700
    style="
      background-image: url('https://img.freepik.com/premium-photo/wedding-flower-backdrop-background-colorful-background-fresh-rose-bunch-flower_41969-9384.jpg?w=2000');
    "
  >
    <div bg-white rounded-lg shadow-lg p-8 backdrop-blur bg-opacity-50>
      <p text-3xl font-bold text-gray-800 mb-4>Do svadby ostáva:</p>
      <div flex justify-center items-center mb-4>
        <div flex flex-col items-center mr-4>
          <div text-4xl font-bold text-gray-800>{{ days }}</div>
          <div text-sm font-bold text-gray-500 uppercase>Dní</div>
        </div>
        <div flex flex-col items-center mr-4>
          <div text-4xl font-bold text-gray-800>{{ hours }}</div>
          <div text-sm font-bold text-gray-500 uppercase>Hodín</div>
        </div>
        <div flex flex-col items-center mr-4>
          <div text-4xl font-bold text-gray-800>{{ minutes }}</div>
          <div text-sm font-bold text-gray-500 uppercase>Minút</div>
        </div>
        <div flex flex-col min-w-75px>
          <div text-4xl font-bold text-gray-800>{{ seconds }}.{{ tenths }}</div>
          <div text-sm font-bold text-gray-500 uppercase>Sekúnd</div>
        </div>
      </div>
      <p text-lg text-center font-bold text-gray-800>1. September 2023 15:00</p>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";

export default {
  setup() {
    const targetDate = new Date("September 1, 2023 15:00:00").getTime();
    const countdownInterval = ref(null);
    const countdown = {
      days: ref(0),
      hours: ref(0),
      minutes: ref(0),
      seconds: ref(0),
      tenths: ref(0),
    };

    const startCountdown = () => {
      countdownInterval.value = setInterval(() => {
        const now = new Date().getTime();
        const distance = targetDate - now;

        if (distance < 0) {
          clearInterval(countdownInterval.value);
          return;
        }

        countdown.days.value = Math.floor(distance / (1000 * 60 * 60 * 24));
        countdown.hours.value = Math.floor(
          (distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
        );
        countdown.minutes.value = Math.floor(
          (distance % (1000 * 60 * 60)) / (1000 * 60)
        );
        countdown.seconds.value = Math.floor((distance % (1000 * 60)) / 1000);
        countdown.tenths.value = Math.floor((distance % 1000) / 100);
      }, 100);
    };

    onMounted(() => {
      startCountdown();
    });

    onBeforeUnmount(() => {
      clearInterval(countdownInterval.value);
    });

    return {
      days: countdown.days,
      hours: countdown.hours,
      minutes: countdown.minutes,
      seconds: countdown.seconds,
      tenths: countdown.tenths,
    };
  },
};
</script>
