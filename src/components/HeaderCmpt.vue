<template>
  <header>
    <div class="l-centent">
      <el-button @click="collapseMenu" type="primary" icon="el-icon-menu" size="mini"></el-button>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item :to="current.path" v-if="current">{{ current.label }}</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="r-content">
      <el-dropdown>
        <span class="el-dropdown-link">
          <img :src="userImg" />
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>个人中心</el-dropdown-item>
          <el-dropdown-item>退出</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </header>
</template>

<script type="text/javascript">
import { mapState } from "vuex";
export default {
  computed: {
    ...mapState({
      current: state => state.HanderTab.currentMenu
    })
  },
  data() {
    return {
      userImg: require("../assets/logo.png")
    };
  },
  methods: {
    collapseMenu() {
      this.$store.commit("collapseMenu");
    }
  }
};
</script>

<style lang="scss" scoped>
header {
  height: 100%;
  background-color: #4b4b4b;
  color: #fff;
  padding: 0 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.l-centent {
  display: flex;
  align-items: center;
}
.r-content {
  img {
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }
}
</style>

<style lang="scss">
.el-breadcrumb__item,
.el-breadcrumb__inner {
  color: #666;
  font-weight: normal;
}
.el-breadcrumb {
  padding-left: 20px;
}
.el-breadcrumb__item:last-child .el-breadcrumb__inner {
  color: #f4f4f4;
}
</style>
