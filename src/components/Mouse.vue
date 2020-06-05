<template>
  <div>
      <span
        class="mouse" :class="{
          smoothTransition: isSmooth,
        }"
        :style="{
          transform,
        }"
      >
        {{isFriday}}
      </span>
  </div>
</template>

<script>
import { ref } from "@vue/composition-api";
export default {
  props: ['transform', 'isSmooth'],
  setup() {
    const isFriday = ref("NO")

    const date = new Date();
    const dayofWeek = date.toLocaleDateString("nl-NL", { weekday: "long" })
    
    if (dayofWeek === "vrijdag") {
      isFriday.value = "YES";
    }

    return{
      isFriday,
    }
  },
};
</script>

<style scoped>
.mouse {
  position: absolute;
  top: 0;
  left: 0;
  transform-origin: center;
  cursor: none;
  transition: transform 1ms ease-in-out;
  z-index: 99;
}

.smoothTransition {
   transition: transform 500ms /* cubic-bezier(0.175, 0.885, 0.32, 1.275); */
}
</style>
