<template>
  <div id="destination-list">
    <div v-for="(item, index) in destinations" v-bind:key="index">
      {{index + 1}}.
      {{item.destination}}
      <button @click="increment(index)">投票する</button>
      <button @click="deleteList(index)">削除する</button>
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
      destinations: []
    };
  },
  methods: {
    increment: function(index, key) {
      // this.destinations[index].count += 1;

      firebase
        .database()
        .ref('sg/dest/')
        .update({
          count: (this.destinations[index].count += 1)
        });
    },
    deleteList: function(index) {
      // this.destinations.splice(index, 1);
      firebase
        .database()
        .ref('sg/dest/' + this.getKey(index))
        .remove();
    },
    listen: function() {
      // dbにデータが存在する場合はデータを取得する
      if (true) {
        console.log('listening…');
        firebase
          .database()
          .ref('sg/dest/')
          .on('value', snapshot => {
            if (snapshot) {
              const rootList = snapshot.val();
              let list = [];
              Object.keys(rootList).forEach((val, key) => {
                rootList[val].id = val;
                list.push(rootList[val]);
              });
              this.destinations = list;
            }
            console.log(this.destinations.length);
          });
      }
    },
    getKey: function(index) {
      console.log(`id:${this.destinations[index].id}`);
      return this.destinations[index].id;
    }
  },
  created() {
    EventBus.$on('destination-add', destination => {
      console.log('destination-add ok');
      // this.destinations.push({
      //   destination: destination,
      //   count: 0
      // });
      if (this.destinations !== null) {
        firebase
          .database()
          .ref('sg/dest/')
          .push({
            destination: destination,
            count: 0,
            date: Date.now()
          });
      }
    });
    this.listen();
  }
};
</script>
