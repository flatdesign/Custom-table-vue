<template>
  <div class="wrapper">
    <table>
      <tr>
        <th v-for="(value, key) in getFilters" :key="key" @click="sort(key)">
          {{key}}
          <span class="fa" :class="getArrowClass(key)"></span>
        </th>
      </tr>
      <tr v-for="(row, index) in getTableArray" :key="index">
        <td v-for="(value, key) in row" :key="key">
          {{value}}
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
  export default {
    props: ['data'],
    data() {
      return {
        filters: [],
        tableArray: [],
      }
    },
    computed: {
      getTableArray() {
        return this.tableArray = this.data.slice();
      },
      getFilters() {
        this.filters = [];
        let obj = {};

        for (let key in this.data.slice()[0]) {
          obj[key] = 'no-sort'
        }
        this.filters.push(obj);
        return this.filters[0];
      },

    },
    methods: {
      sort(key) {
        for (let setting in this.getFilters) {
          if(setting != key) {
            this.getFilters[setting] = 'no-sort';
          }
        }

        switch(this.getFilters[key]) {
          case 'up': {
            this.getFilters[key] = 'no-sort';
            for (let i = 0; i < this.data.length; i++) {
              this.tableArray[i] = this.data[i];
            }
            break;
          }
          case 'down': {
            this.getFilters[key] = 'up';
            this.tableArray.sort((prev, next) => {
              if(+prev[key] || +next[key]) {
                return next[key] - prev[key];
              } else {
                if(next[key].length > prev[key].length) {
                  return 1;
                } else if (next[key].length < prev[key].length) {
                  return -1;
                } else {
                  return next[key].localeCompare(prev[key]);
                }
              }
            });
            break;
          }
          case 'no-sort': {
            this.getFilters[key] = 'down';
            this.tableArray.sort((prev, next) => {
              if(+prev[key] || +next[key]) {
                return prev[key] - next[key];
              } else {
                if(prev[key].length > next[key].length) {
                  return 1;
                } else if (prev[key].length < next[key].length) {
                  return -1;
                } else {
                  return prev[key].localeCompare(next[key]);
                }
              }
            });
            break;
          }
        }
      },
      getArrowClass(key) {
        switch(this.getFilters[key]) {
          case 'up': {
            return ['fa-arrow-up'];
            break;
          }
          case 'down': {
            return ['fa-arrow-down'];
            break;
          }
          case 'no-sort': {
            return ['fa-arrows-v'];
            break;
          }
        }
      }
    }
  }
</script>

<style>
  th:hover {
    cursor: pointer;
  }
  table {
    width: 80%;
    border-collapse: collapse;
    margin-top: 20px;
    margin-bottom: 30px;
  }
  th, td {
    border: 1px solid lightgray;
    padding: 8px 15px;
  }

</style>


