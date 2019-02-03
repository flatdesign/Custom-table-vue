<template>
  <div class="container">
    <div class="row">
      <div class="col-8 col-sm-4">
        <label for="exampleSelect1">Выбрать данные</label>
        <select class="form-control" id="exampleSelect1">
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <option>4</option>
          <option>5</option>
        </select>
        <button type="button" class="btn btn-success" @click.prevent="loadData">Загрузить</button>
      </div>

      <my-table :data="getTableContent"></my-table>
    </div>
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

 .select {
   margin-bottom: 15px;
 }
</style>
