// I am importing `Vue` here and in `main.js`. Should the data be passed in from
// `main.js` to avoid this?

// The data renders as expected in any of the lifecycle methods `beforeCreate`,
// `created`, `beforeMount` and `mounted`. Which one should it go inside?

// Should `getActiveClass` go inside the `computed` object as per the docs page?

// User Story: I can see a table of the Free Code Camp campers who've earned the most brownie points in the past 30 days.

// User Story: I can see how many brownie points they've earned in the past 30 days, and how many they've earned total.

// User Story: I can toggle between sorting the list by how many brownie points they've earned in the past 30 days and by how many brownie points they've earned total.
<template>
  <div id="app">
    <button @click="sortByRecent">Sort by Recent Points</button>
    <button @click="sortByAllTime">Sort by All Time Points</button>
    <table>
      <thead>
        <tr>
          <td>#</td>
          <td></td>
          <td>Username</td>
          <td :class="getActiveClass('recent')">Recent Points</td>
          <td :class="getActiveClass('allTime')">Total Points</td>
        </tr>
      </thead>
      <tr v-for="(user, index) in data">
        <td>{{index + 1}}</td>
        <td>
          <img :src="user.img">
        </td>
        <td>{{user.username}}</td>
        <td>{{user.recent}}</td>
        <td>{{user.alltime}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import Vue from 'vue'
import _ from 'lodash'

Vue.use(require('vue-resource'));

const endpoint = 'https://fcctop100.herokuapp.com/api/fccusers/top/recent'

const store = {
  state: {
    sortingBy: ''
  }
}

export default {
  data() {
    return {
      data: [],
      state: store.state
    }
  },
  mounted() {
    Vue.http.get(endpoint).then(response => {
      this.data = response.body
    }, response => {
      console.warn('There was a data error:', response)
    })
  },
  methods: {
    sortByRecent() {
      this.data = _.sortBy(this.data, 'recent').reverse()
      store.state.sortingBy = 'recentDescending';
    },
    sortByAllTime() {
      this.data = _.sortBy(this.data, 'alltime').reverse()
      store.state.sortingBy = 'allTimeDescending';
    },
    getActiveClass(string) {
      return {active: store.state.sortingBy.includes(string)}
    }
  }
}
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.active {
  font-weight: bold;
}
button {
  font-size: 20px;
}
img {
  border-radius: 50%;
  height: 30px;
  width: 30px;
}
</style>
