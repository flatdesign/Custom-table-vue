<template>
  <div class="wrapper">
      <div class="col-8 col-sm-4">
        <div class="form-group controls">
          <input type="text" class="form-control" placeholder="Поиск...">
          <input type="number" class="form-control" value="20" min="1" max="50" v-model="userRowCount">
          <button type="button" class="btn btn-success" @click.prevent="setRows">Применить</button>
        </div>
      </div>

      <table>
        <tr>
          <th v-for="(value, key) in getFilters" :key="key" @click="sort(key)">
            {{key}}
            <span class="fa" :class="getArrowClass(key)"></span>
          </th>
        </tr>
        <tr v-for="(row, index) in getCurrentRows" :key="index">
          <td v-for="(value, key) in row" :key="key">
            {{value}}
          </td>
        </tr>
      </table>

      <nav aria-label="Page navigation">
        <ul class="pagination">
          <li class="page-item">
            <a class="page-link" href="#" aria-label="Previous" @click.prevent="prevPage">
              <span aria-hidden="true">&laquo;</span>
              <span class="sr-only">Previous</span>
            </a>
          </li>
          <li class="page-item" v-for="(page, index) in getPageCount" :key="index" :class="getPageClass(index)">
            <a class="page-link" href="#" @click.prevent="setPage(index)">{{index + 1}}</a>
          </li>
          <li class="page-item">
            <a class="page-link" href="#" aria-label="Next"  @click.prevent="nextPage">
              <span aria-hidden="true">&raquo;</span>
              <span class="sr-only">Next</span>
            </a>
          </li>
        </ul>
      </nav>
  </div>
</template>

<script>
  export default {
    props: ['data'],
    data() {
      return {
        filters: [],
        tableArray: [],
        rowCount: 10,
        userRowCount: 0,  // Выбор пользователя
        currentPage: 0,
      }
    },
    computed: {
      getPageCount() {
        return Math.ceil(this.data.length / this.rowCount);
      },
      getCurrentRows() {
        console.log(1);
        return this.tableArray.slice(this.rowCount * this.currentPage, this.rowCount * this.currentPage + this.rowCount);
      },
      getTableArray() {
        return this.tableArray = this.data.slice();
      },
      getFilters() {
        console.log(1);
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
      getPageClass(index) {
        if(index == this.currentPage)
          return 'active'
        else
          return ''
      },
      prevPage() {
        if(this.currentPage > 0)
          this.currentPage--
      },
      nextPage() {
        if(this.currentPage < this.getPageCount - 1)
          this.currentPage++
      },
      setPage(index) {
        this.currentPage = index;
      },
      setRows() {
        if(+this.userRowCount < 1) {
          this.rowCount = 1;
          this.userRowCount = 1;
        } else if(+this.userRowCount > 50) {
          this.rowCount = 50;
          this.userRowCount = 50;
        } else {
          this.rowCount = +this.userRowCount;
        }
        return;
      },
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


