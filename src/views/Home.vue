<template>
  <div class="wrapper">
    <div class="search">
      <label for="search">Search </label>
      <input id="search" type="text" name="search" v-model="searchValue" @input="handleInput">
    </div>

    <div v-for="item in result" :key="item.links" >
      <p>{{ item.links[0].href}}</p>
      <!-- <p>{{ getLink(item.links[0].href) }}</p> -->
      <!-- <img  src="" alt="Doesnt work">  -->
      <img  v-bind:src="item.links[0].href" alt="Doesnt work">
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov/search?q=';
export default {
  name: 'Home',
  data() {
    return {
      searchValue: '',
      result: [],
      link: '',
    };
  },

  methods: {
    handleInput: debounce(function () {
      axios.get(`${API}${this.searchValue}&media_type=image`)
        .then((response) => {
          this.result = response.data.collection.items;
        });
      // .catch((error) => {
      //   console.log(error);
      // });
    }, 500),
    getLink(a) {
      this.link = a;
    },
  },
};

</script>
<style scoped>

  .wrapper {
    display:flex;
    flex-direction: column;
    align-items: center;

    margin: 0;
    padding: 30px;
    width: 100%;

    /* background-color: rgb(31, 4, 4); */
  }

  .search{
    display: flex;
    flex-direction: column;
    width:300px;
    /* //color: white; */
    font-size: 26px;
  }

  input {
    height: 30px;
    border: 0;
    border-bottom: solid black 1px;
  }
</style>
