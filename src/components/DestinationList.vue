<template>
  <div id="destination-list">
    <div v-for="(item, index) in destinations" v-bind:key="index">
      {{index + 1}}.
      {{item.destination}}
      <button @click="increment(index)">投票する</button>
      <button @click="deleteList(index)">削除する</button>
      {{item.count}}
    </div>
    <div>残り{{limitCount - userCount}}回</div>
  </div>
</template>

<script>
import { EventBus } from './event-bus.js';

export default {
  name: 'destination-list',
  props: ['userData'],
  data() {
    return {
      destinations: [],
      userCount: 0,
      limitCount: 3
    };
  },
  methods: {
    increment: function(index, key) {
      // this.destinations[index].count += 1;
      if (this.limitCount - this.userCount > 0) {
        firebase
          .database()
          .ref('sg/dest/' + this.getKey(index))
          .update({
            count: (this.destinations[index].count += 1)
          });

        console.log(
          `this.destinations.count:${this.destinations[index].count}`
        );

        // this.userCount += 1;
        // EventBus.$emit('increment-user-count', this.userCount);
        console.log(this.userData.uid);

        firebase
          .database()
          .ref('sg/user/' + this.userData.uid)
          .update({ count: (this.userCount += 1) });
      }
    },
    deleteList: function(index) {
      // this.destinations.splice(index, 1);
      firebase
        .database()
        .ref('sg/dest/' + this.getKey(index))
        .remove();
    },
    listen: function() {
      console.log('listening…');
      firebase
        .database()
        .ref('sg/dest/')
        .on('value', snapshot => {
          // dbにデータが存在する場合はデータを取得する
          if (snapshot.val() !== null) {
            const rootList = snapshot.val();
            let list = [];
            Object.keys(rootList).forEach((val, key) => {
              rootList[val].id = val;
              list.push(rootList[val]);
            });
            this.destinations = list;
            console.log(`destinations.length:${this.destinations.length}`);
          } else {
            this.destinations = [];
            console.log(`destinations.length:${this.destinations.length}`);
          }
        });
      firebase
        .database()
        .ref('sg/user/' + this.userData.uid)
        .on('value', snapshot => {
          // console.log(snapshot.val());
          const rootCount = snapshot.val();
          console.log(rootCount.count);
          this.userCount = rootCount.count;

          // this.userCount = count + 1;
          // console.log(userCount);
        });
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
