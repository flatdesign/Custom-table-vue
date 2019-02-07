<template>
  <div class="container">
    <div class="row">
      <div class="col-8 col-sm-4">
        <label for="select">Выбрать данные</label>
        <select class="form-control" id="select" v-model="target">
          <option value="posts">Посты</option>
          <option value="comments">Комменты</option>
          <option value="albums">Альбомы</option>
          <option value="photos">Фото</option>
          <option value="todos">Задачи</option>
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
        target: 'posts',
        urlAPI: 'https://jsonplaceholder.typicode.com/albums',
        tableContent: []
      }
    },
    computed: {
      getTableContent() {
        return this.tableContent;
      },
      getURL() {
        return 'https://jsonplaceholder.typicode.com/' + this.target
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
        this.makeRequest(this.getURL).then(responce => this.tableContent = responce.data);
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
