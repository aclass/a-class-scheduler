<template>
  <div id="add-list">
    <h1>{{msg}}</h1>
    <form @submit.prevent="destinationAdd">
      <input type="text" placeholder="場所" ref="destination">
      <button type="submit">追加</button>
    </form>
  </div>
</template>
<script>
import { EventBus } from './event-bus.js';

export default {
  name: 'add-list',
  data() {
    return {
      msg: 'hello!'
    };
  },
  methods: {
    destinationAdd: function(event, value) {
      const destination = this.$refs.destination.value;
      EventBus.$emit('destination-add', destination);
    }
  }
};
</script>
