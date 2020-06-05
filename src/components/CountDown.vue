<template>
  <div>
    <div v-if="ready">
    <p class="yes-no">{{isFriday}}</p>
    <p class="time-left">{{timeLeft}}</p>
    </div>
    <p class="time-left">{{currentTime}}</p>
  </div>
</template>

<script>
import { ref } from "@vue/composition-api";

export default {
  setup() {
    const ready = ref(false)

    const timeLeft = ref(new Date());
    const isFriday = ref("NO");

    const currentTime = ref('Loading...')
    
    

    setInterval(() => {
      const date = new Date();
      const resultDate = new Date(date.getTime());
      const dayOfWeek = 5; // 5 = friday (zero based)
      resultDate.setDate(
        date.getDate() + ((7 + dayOfWeek - date.getDay() - 1) % 7) + 1
      ); // calc the next friday

      
      resultDate.setHours(0, 0, 0, 0); 

      
      const options = { month: "short", year: "numeric", day: "numeric" };
      const nextFriday = new Date(
        resultDate.toLocaleTimeString("en-US", options)
      );
      const countDownDate = nextFriday.getTime();

      const weekDay = date.toLocaleDateString("nl-NL", { weekday: "long" });

      const now = new Date().getTime();

      const waitTime = countDownDate - now;
      
      const days = Math.floor(waitTime / (1000 * 60 * 60 * 24));
      let hours = Math.floor(
        (waitTime % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
      );
      let minutes = Math.floor((waitTime % (1000 * 60 * 60)) / (1000 * 60));
      let seconds = Math.floor((waitTime % (1000 * 60)) / 1000);
      if (minutes.toString().length < 2) minutes = "0" + minutes;
      if (hours.toString().length < 2) hours = "0" + hours;
      if (seconds.toString().length < 2) seconds = "0" + seconds;

      if (weekDay === "vrijdag") {
        const options = { year: "numeric", month: "short", day: "numeric" };
        const fridayCurrent = new Date();
        const lastFridayMoment = new Date(
          fridayCurrent.toLocaleTimeString("en-US", options)
        );
        lastFridayMoment.setHours(22, 59, 59, 99);
        const a = new Date(lastFridayMoment).getTime();
        const b = new Date().getTime();
        const calcLeft = a - b;
        const offSet = calcLeft - 60000;
        const fridayLeft = new Date(offSet);

        isFriday.value = "YES";
        timeLeft.value = `It is friday for the next: ${fridayLeft.toLocaleTimeString(
          [],
          { hour12: false }
        )}`;
      } else {
        timeLeft.value = `The next friday is in ${days}day(s) - ${hours}:${minutes}:${seconds}`;
      }
      currentTime.value = ''
      ready.value = true
      // timeLeft.value = timeLeft;
    }, 1000);

    return {
      currentTime,
      isFriday,
      timeLeft,
      ready
    };
  }
};
</script>

<style scoped>
.yes-no {
  margin: 2rem;
  font-size: calc(2rem + 8vmin);
  text-align: center;
  justify-content: center;
}

.question,
.time-left {
  text-align: center;
  font-size: calc(1rem + 4vmin);
}
</style>
