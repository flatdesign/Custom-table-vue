<template>
  <div class="container">
    <button type="button" class="btn btn-success" @click.prevent="loadData">Загрузить данные</button>
    <my-table :data="getTableContent"></my-table>
  </div>
</template>

<script>
  import table from './components/Table.vue';
  import axios from 'axios';

  export default {
    name: 'app',
    components: {
      'my-table': table
    },
    data () {
      return {
        msg: 'Hello Danil',
        urlAPI: 'https://jsonplaceholder.typicode.com/albums',
        tableContent: []
      }
    },
    computed: {
      getTableContent() {
        return this.tableContent;
      }
    },
    methods: {
      async makeRequest(url) {
        try {
          let responce = await axios.get(url);
          return responce;
        } catch (e) {
          console.log(e);
          alert('Не удалось загрузить данные с сервера');
        }
      },
      loadData() {
        this.makeRequest(this.urlAPI).then(responce => this.tableContent = responce.data);
      }

    }
  }
</script>

<style>
 .container {
    padding-top: 50px;
 }
</style>
