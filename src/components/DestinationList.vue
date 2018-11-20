<template>
  <div id="destination-list">
    <div v-for="(item, index) in destinations" v-bind:key="index">
      {{item.id}}.
      {{item.destination}}
      <button @click="increment(index)">投票する</button>
      {{item.count}}
    </div>
  </div>
</template>

<script>
export default {
  name: 'destination-list',
  data() {
    return {
      destinations: [
        { id: 1, destination: '金沢', count: 0 },
        { id: 2, destination: '札幌', count: 0 },
        { id: 3, destination: '名古屋', count: 0 },
        { id: 4, destination: '熊本', count: 0 }
      ]
    };
  },
  methods: {
    increment: function(index) {
      this.destinations[index].count += 1;
    }
  }
};
</script>
