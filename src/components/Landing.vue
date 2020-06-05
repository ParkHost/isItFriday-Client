<template>
<div class="landing">
  <count-down />
  <!-- passes 'socket' to child component, so it can do his own rendering -->
  <mouse-listener :socket="socket" /> 
  <client-state :socket="socket"/>
</div>
</template>

<script>
import io from "socket.io-client";

import CountDown from './CountDown.vue';
import MouseListener from './MouseListener.vue';
import ClientState from './ClientState.vue';

import API_URL from "../API_URL";

export default {
  components: {
    CountDown,
    MouseListener,
    ClientState,
  },
  setup() {
    const socket = io(API_URL);

    return {
      socket, 
    };
  },
};
</script>

<style scoped>
.landing {
  widows: 100%;
  height: 100%;
  background: black;
  color: white;
  font-family: sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

</style>
