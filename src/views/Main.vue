<template>
  <el-container style="height:100%">
    <el-aside width="auto">
      <aside-cmpt></aside-cmpt>
    </el-aside>
    <el-container>
      <el-header>
        <header-cmpt></header-cmpt>
      </el-header>
      <tab-cmpt></tab-cmpt>
      <el-main>
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script type="text/javascript">
import HeaderCmpt from "../components/HeaderCmpt";
import AsideCmpt from "../components/AsideCmpt";
import TabCmpt from "../components/TabCmpt";
export default {
  components: {
    HeaderCmpt,
    AsideCmpt,
    TabCmpt
  },
  data() {
    return {};
  },
  methods: {}
};
</script>

<style lang="scss" scoped>
.el-header,
.el-main {
  padding: 0;
}
</style>
