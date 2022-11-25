<template>
  <div >
    <el-input size="large" placeholder="书名" v-model=title @input="handleSearch" @change="handleSearch"
      @click="handleSearch" />
    <el-input size="large" placeholder="作者" v-model=author @input="handleSearch" @change="handleSearch"
      @click="handleSearch" />
    <el-input size="large" placeholder="出版社" v-model=publisher @input="handleSearch" @change="handleSearch"
      @click="handleSearch" />
    <el-input size="large" placeholder="扩展名" v-model=extension @input="handleSearch" @change="handleSearch"
      @click="handleSearch" />
    <el-input size="large" placeholder="语言" v-model=language @input="handleSearch" @change="handleSearch"
      @click="handleSearch" />
    <el-input size="large" placeholder="ISBN" v-model=isbn @input="handleSearch" @change="handleSearch"
      @click="handleSearch" />

    <el-input size="large" placeholder="复杂查询" v-model=query @input="handleSearch" @change="handleSearch"
      @click="handleSearch" />

    <div style="height: 900px">
      <el-auto-resizer>
        <template #default="{ height, width }">
          <el-table-v2 :columns="columns" :data="data"
          :estimated-row-height="50"
          :cache="10"
          :width="width" :height="height"
          >
          </el-table-v2>
        </template>
      </el-auto-resizer>
    </div>
  </div>
</template>

<script>
import { TableV2FixedDir } from 'element-plus'

export default {
  title: 'zlib-searcher',
  setup() {
  },
  methods: {
    handleSearch: function () {
      this.$http.get("search?limit=100&query=" + this.constructQuery()).then((response) => {
        this.data = response.data.books;
      }).catch(() => { });
    },
    constructQuery: function () {
      if (this.query) {
        return this.query;
      }

      var query = '';
      if (this.title) {
        this.title.split(' ').forEach((e) => {
          if (e) {
            query = query + 'title:"' + e + '"';
          }
        });
      }
      if (this.author) {
        this.author.split(' ').forEach((e) => {
          if (e) {
            query = query + 'author:"' + e + '"';
          }
        });
      }
      if (this.publisher) {
        this.publisher.split(' ').forEach((e) => {
          if (e) {
            query = query + 'publisher:"' + e + '"';
          }
        });
      }
      if (this.extension) {
        query = query + 'extension:"' + this.extension + '"';
      }
      if (this.language) {
        query = query + 'language:"' + this.language + '"';
      }

      if (this.isbn) {
        query = query + 'isbn:"' + this.isbn + '"';
      }

      console.log(query);
      return query;
    }
  },
  data() {
    return {
      query: '',
      title: '',
      author: '',
      publisher: '',
      extension: '',
      language: '',
      isbn: '',
      columns: [
        {
          title: 'zlib|libgen id',
          key: 'id',
          dataKey: 'id',
          width: 140,
        },
        {
          title: '书名',
          key: 'title',
          dataKey: 'title',
          width: 400,
        },
        {
          title: '作者',
          key: 'author',
          dataKey: 'author',
          width: 350,
        },
        {
          title: '出版社',
          key: 'publisher',
          dataKey: 'publisher',
          width: 300,
        },
        {
          title: '扩展名',
          key: 'extension',
          dataKey: 'extension',
          width: 100,
          align: 'center',
        },
        {
          title: '文件大小',
          key: 'filesize',
          dataKey: 'filesize',
          width: 120,
        },
        {
          title: '语言',
          key: 'language',
          dataKey: 'language',
          width: 120,
          align: 'center',
        },
        {
          title: '年份',
          key: 'year',
          dataKey: 'year',
          width: 100,
          align: 'center',
        },
        {
          title: '页数',
          key: 'pages',
          dataKey: 'pages',
          width: 100,
          align: 'center',
        },
        {
          title: 'ISBN',
          key: 'isbn',
          dataKey: 'isbn',
          width: 200,
        },
        {
          title: 'IPFS CID',
          key: 'ipfs_cid',
          dataKey: 'ipfs_cid',
          width: 480,
          fixed: TableV2FixedDir.RIGHT,
        },
      ],
      data: []
    }
  }
}
</script>

<style scoped>

</style>
