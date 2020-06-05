<template>
  <mouse :transform="transform" />
</template>

<script>
import { ref, reactive, computed } from "@vue/composition-api";

import Mouse from './Mouse.vue'


export default {
  props: ['socket'], //receiving the 'socket' prop from parent component 'Landing.vue'
  components: {
    Mouse,
  },
  setup( { socket }) { // gets 'socket' out of parent component 'Landing'
    function clamp(num, min, max) {
      return num <= min ? min : num >= max ? max : num;
    }
    const isFriday = ref("NO")

    const date = new Date();
    const dayofWeek = date.toLocaleDateString("nl-NL", { weekday: "long" })
    
    if (dayofWeek === "vrijdag") {
      isFriday.value = "YES";
    }

    const location = reactive({
      x: window.innerWidth / 2,
      y: window.innerHeight / 2
    });

    const lastLocation = reactive({
      x: location.x,
      y: location.y
    });

    const transform = computed(
      () => `translate(-50%, -50%) translate(${location.x}px, ${location.y}px)`
    );

    document.addEventListener("mousemove", (event) => {
      location.x = event.x;
      location.y = event.y;
    });
    
    function updateLocation() {
      if (
        Math.abs(location.x - lastLocation.x) > 0.001 ||
        Math.abs(location.y - lastLocation.y) > 0.001 
      ) {
        lastLocation.x = location.x;
        lastLocation.y = location.y;
        socket.emit("location", {
          x: clamp(location.x / window.innerWidth, 0, 1),
          y: clamp(location.y / window.innerHeight, 0, 1)
        });
      }
      setTimeout(updateLocation, 500);
    }

    socket.on("connect", () => {
      updateLocation();
    });
    
    // socket.on("connect", () => {
    //   console.log(socket.id)
    // });

    return {
      location,
      transform,
      isFriday,
    };
  }
};
</script>



