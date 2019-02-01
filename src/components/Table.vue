<template>
  <div class="wrapper">
    <table>
      <tr>
        <th v-for="(value, key) in getFilters" :key="key" @click="sort">
          {{key}}<span :class=" getFilters[key] == 'down' ? 'fa fa-sort-desc' : 'fa-sort-asc'"></span>
        </th>
      </tr>
      <tr v-for="(row, index) in data" :key="index">
        <td v-for="(elem, index) in row" :key="index">
          {{elem}}
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
        filters: []
      }
    },
    computed: {
      getFilters() {
        let obj = {};
        this.filters = [];

        for (let key in this.data[0]) {
          obj[key] = 'down'
        }
        this.filters.push(obj);
        return this.filters[0];
      }
    },
    methods: {
      sort() {
        console.log('sort');
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


