<template>
  <div id="app">
    <img src="./assets/logo.png">
    <!-- <h1>{{ msg }}</h1> -->
    <div>
      <span v-if="isLogin">Name: {{userData.displayName}}</span>
      <button v-if="isLogin" @click="logout">ログアウト</button>
    </div>
    <DestinationList v-if="isLogin" :userData="userData"></DestinationList>
    <AddList v-if="isLogin"></AddList>
    <Home v-if="!isLogin"></Home>
  </div>
</template>

<script>
import { EventBus } from './components/event-bus.js';
import DestinationList from './components/DestinationList';
import AddList from './components/AddList';
import Home from './components/Home';

export default {
  name: 'app',
  data() {
    return {
      msg: 'A-CLASS SCHEDULER',
      isLogin: false,
      userData: null
    };
  },
  methods: {
    logout: function() {
      firebase.auth().signOut();
    }
  },
  created: function() {
    firebase.auth().onAuthStateChanged(user => {
      console.log(user);
      if (user) {
        this.isLogin = true;
        this.userData = user;
      } else {
        this.isLogin = false;
        this.userData = null;
      }
    });

    EventBus.$on('increment-user-count', userCount => {
      console.log('成功');
      userCount: userCount += 1;
      console.log(userCount);

      // firebase.database().ref('sg/' + userData.displayName)
    });
  },
  components: {
    DestinationList: DestinationList,
    AddList: AddList,
    Home: Home
  }
};
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
