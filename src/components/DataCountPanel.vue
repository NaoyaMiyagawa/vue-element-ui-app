<template>
  <el-row class="count-row" :gutter="2">
    <el-col :span="4" :offset="20">
      <el-input size="mini" :disabled="true" :value="dataCount">
        <template slot="prepend">全体件数</template>
      </el-input>
    </el-col>
  </el-row>
</template>

<script>
export default {
  name: 'DataCountPanel',
  props: {
    dataCount: {
      type: Number,
      default: 0,
    },
    selectedCount: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style lang="scss" scoped>
.count-row {
  margin-top: 10px;
}
</style>
