<template>
  <div id="add-list">
    <form @submit.prevent="destinationAdd">
      <input v-model="destination" type="text" placeholder="場所" ref="destination">
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
      destination: ''
    };
  },
  methods: {
    destinationAdd: function(event, value) {
      const destination = this.$refs.destination.value;
      if (destination == '') {
        return;
      }
      this.destination = '';
      EventBus.$emit('destination-add', destination);
    }
  }
};
</script>
