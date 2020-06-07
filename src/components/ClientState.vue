<template>
  <div>
    <mouse
      v-for="client in clients"
      :isSmooth="true"
      :transform="client.transform"
      :key="client.id" 
    />
  </div>
</template>

<script>
 import { ref } from '@vue/composition-api';

 import Mouse from './Mouse.vue'

export default {
  components: {
    Mouse,
  },
  props: ['socket'], //receiving the 'socket' prop from parent component 'Landing.vue'
  setup( { socket }) { // passes 'socket' into the setup
    const clients = ref([]);
    socket.on("state", state => {     
      // What is Object.entries? Loops over the object and gets each entry and put's it into an array.
      // What are the parameters with reduce? deconstructing array?
      clients.value = Object.entries(state).reduce((all, [id, client]) => {
        if (id !== socket.id) {
          all.push({
            id,
            transform: `translate(-50%, -50%) translate(${client.x * 100}vw, ${client.y * 100}vh)`,
          });
        }
        return all;
      }, []);
    });

    return {
      clients,
    };
  },
};
</script>

<style>

</style>
