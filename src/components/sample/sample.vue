<template>
  <div id="choices">
    <div>
      <input type="text" v-model="inputPlace" placeholder="入力してね">
      <button v-on:click="addPlace">追加</button>
    </div>
    <div>
      <ol>
        <li v-for="(place, index) in places">
          {{ place.name }}（{{ place.count }}）
          <button v-on:click="countUp(index)">いいね</button>
          <button v-on:click="deletePlace(index)">選択肢から削除</button>
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
export default {
  name: 'choices',
  data() {
    return {
      inputPlace: '',
      places: []
    };
  },
  created: function () {
    this.test();
  },
  methods: {
    test: function () {
      database.ref('yoshitani').on('value', placesData => {
        if (placesData.val() !== null) {
          const data = placesData.val();
          const array = [];
          Object.keys(data).forEach((place) => {
            array.push({
              name: place,
              count: data[place].count
            })
          })
          this.places = array;
        } else {
          this.places = [];
        }
      })
    },
    addPlace: function () {
      if (this.inputPlace.length > 0) {
        database.ref('yoshitani/' + this.inputPlace).set({
          count: 0
        });
        this.inputPlace = '';
      }
    },
    deletePlace: function (index) {
      database.ref('yoshitani/' + this.places[index].name).remove();
    },
    countUp: function (index) {
      const count = this.places[index].count + 1;
      database.ref('yoshitani/' + this.places[index].name).update({
        count: count
      })
    }
  }
}
</script>
