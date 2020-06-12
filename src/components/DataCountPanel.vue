<template>
  <el-row class="count-row" :gutter="2">
    <el-col :span="4" :offset="12">
      <el-button type="plain" plain @click="resetInfiniteScroll">リセット</el-button>
    </el-col>
    <el-col :span="4" :offset="0">
      <el-input :disabled="true" :value="displayDataLength">
        <template slot="prepend">読込件数</template>
      </el-input>
    </el-col>
    <el-col :span="4" :offset="0">
      <el-input :disabled="true" :value="tableDataLength">
        <template slot="prepend">全体件数</template>
      </el-input>
    </el-col>
  </el-row>
</template>

<script>
export default {
  name: 'DataCountPanel',
  props: {
    tableDataLength: {
      type: Number,
      default: 0,
    },
    displayDataLength: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    resetInfiniteScroll() {
      this.$emit('reset-infinite-scroll');
    },
  },
};
</script>

<style lang="scss" scoped>
.count-row {
  margin-top: 10px;
}
</style>
