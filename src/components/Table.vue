<template>
  <div class="table" style="width: 1200px; margin: auto;">
    <el-table
      ref="singleTable"
      :data="tableData"
      id="single-table"
      highlight-current-row
      @current-change="handleCurrentChange"
      max-height="440"
      border
      stripe
    >
      <!-- fixed columns -->
      <el-table-column type="selection" width="50" fixed />
      <el-table-column type="index" width="50" fixed />
      <el-table-column prop="countryName" label="CountryName" width="200" sortable fixed />

      <!-- unfixed columns -->
      <el-table-column prop="countryCode" label="CountryCode" width="150" sortable />
      <el-table-column prop="regionCode" label="RegionCode" width="150" sortable />
      <el-table-column prop="regionName" label="RegionName" width="200" sortable />
      <el-table-column prop="remarks" label="Remarks" width="250" sortable />
      <el-table-column prop="data1" label="Data1" width="100" sortable />
      <el-table-column prop="data2" label="Data2" width="100" sortable />
      <el-table-column prop="data3" label="Data3" width="100" sortable />
      <el-table-column prop="data4" label="Data4" width="100" sortable />
      <el-table-column prop="data5" label="Data5" width="100" sortable />
      <el-table-column prop="data6" label="Data6" width="100" sortable />
      <el-table-column prop="data7" label="Data7" width="100" sortable />
      <el-table-column prop="data8" label="Data8" width="100" sortable />
      <el-table-column prop="data9" label="Data9" width="100" sortable />
      <el-table-column prop="data10" label="Data10" width="100" sortable />
      <el-table-column prop="data11" label="Data11" width="100" sortable />
      <el-table-column prop="data12" label="Data12" width="100" sortable />
      <el-table-column prop="data13" label="Data13" width="100" sortable />
      <el-table-column prop="data14" label="Data14" width="100" sortable />
      <el-table-column prop="data15" label="Data15" width="100" sortable />
      <el-table-column prop="data16" label="Data16" width="100" sortable />
      <el-table-column prop="data17" label="Data17" width="100" sortable />
      <el-table-column prop="data18" label="Data18" width="100" sortable />
      <el-table-column prop="data19" label="Data19" width="100" sortable />
    </el-table>

    <data-count-panel :data-count="dataCount" :selected-count="this.tableData.length" />
  </div>
</template>

<script>
import axios from 'axios';
import DataCountPanel from './DataCountPanel';

export default {
  name: 'Table',
  components: {
    DataCountPanel,
  },
  data() {
    return {
      dataCount: 0,
      tableData: [],
      currentRow: null,
    };
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
      let apiBaseUrl = 'https://opendata.resas-portal.go.jp';
      let apiUrl = apiBaseUrl + '/api/v1/regions/middle';
      // let apiUrl = apiBaseUrl + '/api/v1/regions/middle?regionCode=1';
      const res = await axios.get(apiUrl, {
        headers: { 'X-API-KEY': process.env.VUE_APP_RESAS_API_KEY },
      });

      let dataList = res.data.result;
      dataList.forEach(data => {
        data.data1 = 'data1';
        data.data2 = 'data2';
        data.data3 = 'data3';
        data.data4 = 'data4';
        data.data5 = 'data5';
        data.data6 = 'data6';
        data.data7 = 'data7';
        data.data8 = 'data8';
        data.data9 = 'data9';
        data.data10 = 'data10';
        data.data11 = 'data11';
        data.data12 = 'data12';
        data.data13 = 'data13';
        data.data14 = 'data14';
        data.data15 = 'data15';
        data.data16 = 'data16';
        data.data17 = 'data17';
        data.data18 = 'data18';
        data.data19 = 'data19';
      });

      this.tableData = dataList;
      this.dataCount = dataList.length;
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
