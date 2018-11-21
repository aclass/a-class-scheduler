<template>
  <div id="destination-list">
    <div v-for="(item, index) in destinations" v-bind:key="index">
      {{index + 1}}.
      {{item.destination}}
      <button @click="increment(index)">投票する</button>
      {{item.count}}
    </div>
  </div>
</template>

<script>
import { EventBus } from './event-bus.js';

export default {
  name: 'destination-list',
  data() {
    return {
      destinations: [
        { destination: '金沢', count: 0 },
        { destination: '札幌', count: 0 },
        { destination: '名古屋', count: 0 },
        { destination: '熊本', count: 0 }
      ]
    };
  },
  methods: {
    increment: function(index) {
      this.destinations[index].count += 1;
    }
  },
  mounted() {
    EventBus.$on('destination-add', destination => {
      console.log('ok');
      this.destinations.push({
        destination: destination,
        count: 0
      });
    });
  }
};
</script>
