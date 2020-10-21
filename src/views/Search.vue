<template>
  <div class="wrapper">
    <div class="search">
      <label for="search">Search</label>
      <input type="text" name="search" id="search" v-model="searchValue" @input="handleInput"
      />
      <ul>
        <li v-for="item in results" :key="item.data[0].nasa_id">
          <p> {{ item.data[0].description }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov/search';
export default {
  name: 'search',
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function () {
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>
<style lang="scss" scooped>
    .wrapper {
      width:100%;
      margin: 0;
      display: flex;
      flex-direction: column;
      padding: 30px;
      align-items: center;
    }
    .search {
      width:300px;
      display:flex;
      flex-direction: column;
      label{
         font-family: sans-serif;
      }

      input{
        height: 30px;
        border: 0;
        border-bottom: 1px solid black;
      }
    }

</style>
