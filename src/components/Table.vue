<template>
  <div class="table" style="width: 1000px; margin: auto;">
    <el-table
      ref="singleTable"
      :data="displayData"
      id="single-table"
      highlight-current-row
      @current-change="handleCurrentChange"
      max-height="440"
      border
      stripe
    >
      <!-- unfixed columns -->
      <el-table-column type="index" width="50" />
      <el-table-column prop="countryName" label="CountryName" width="200" sortable />
      <el-table-column prop="countryCode" label="CountryCode" width="150" sortable />
      <el-table-column prop="regionCode" label="RegionCode" width="150" sortable />
      <el-table-column prop="regionName" label="RegionName" width="200" sortable />
      <el-table-column prop="remarks" label="Remarks" width="250" sortable />

      <!-- 動的読み込みスクロール -->
      <infinite-loading
        slot="append"
        :identifier="infiniteScroll.id"
        :distance="1000"
        @infinite="infiniteHandler"
        force-use-infinite-wrapper=".el-table__body-wrapper"
      >
        <span slot="no-results" />
        <span slot="no-more" />
      </infinite-loading>
    </el-table>

    <data-count-panel
      :table-data-length="tableData.length"
      :display-data-length="displayData.length"
      @reset-infinite-scroll="resetInfiniteScroll"
    />
  </div>
</template>

<script>
import axios from 'axios';
import InfiniteLoading from 'vue-infinite-loading';
import DataCountPanel from './DataCountPanel';

export default {
  name: 'Table',
  components: {
    InfiniteLoading,
    DataCountPanel,
  },
  data() {
    return {
      loading: false,
      tableData: [],
      currentRow: null,
      infiniteScroll: {
        id: +new Date(),
        defaultIndex: 30,
        additionalIndex: 5,
        currentIndex: 30,
      },
    };
  },
  computed: {
    displayData() {
      return this.tableData.slice(0, this.infiniteScroll.currentIndex);
    },
  },
  created() {
    this.getDataList();
  },
  methods: {
    setCurrent(row) {
      this.$refs.singleTable.setCurrentRow(row);
    },
    handleCurrentChange(val) {
      this.currentRow = val;
    },
    async getDataList() {
      this.tableData = [];
      let apiBaseUrl = 'https://opendata.resas-portal.go.jp';
      let apiUrl = apiBaseUrl + '/api/v1/regions/middle';

      this.loading = true;
      const res = await axios.get(apiUrl, {
        headers: { 'X-API-KEY': process.env.VUE_APP_RESAS_API_KEY },
      });
      this.loading = false;

      if (!res) return;

      this.tableData = res.data.result;

      this.infiniteScroll.id++;
    },
    // 動的読み込みスクロール
    infiniteHandler($state) {
      if (this.tableData.length && this.tableData.length > this.displayData.length) {
        // 追加読み込み
        this.infiniteScroll.currentIndex += this.infiniteScroll.additionalIndex;
        $state.loaded();
      } else {
        // 全読み込み完了
        $state.complete();
      }
    },
    // 動的読み込みスクロールをリセット
    resetInfiniteScroll() {
      this.infiniteScroll.id++;
      this.infiniteScroll.currentIndex = this.infiniteScroll.defaultIndex;
      document.getElementsByClassName('el-table__body-wrapper')[0].scrollTo({ top: 0 });
    },
  },
};
</script>

<style lang="scss" scoped>
::v-deep #single-table {
  .el-table__body {
    tr.current-row > td {
      &:first-child {
        border-left: 2px solid #ccc;
      }
      &:last-child {
        border-right: 2px solid #ccc;
      }
      background-color: #ddd;
      border-top: 2px solid #ccc;
      border-bottom: 2px solid #ccc;
    }
    tr.hover-row > td {
      background-color: #eee;
    }
  }
}
</style>
