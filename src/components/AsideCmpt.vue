/* 二次封装element-ui的导航菜单 */
<template>
  <el-menu
    :collapse="isCollpase"
    default-active="2"
    class="el-menu-vertical-demo"
    background-color="#545c64"
    text-color="#fff"
    active-text-color="#ffd04b"
  >
    <el-menu-item
      :index="item.path"
      v-for="item in onChildren"
      :key="item.path"
      @click="MenuClick(item)"
    >
      <i :class="'el-icon-'+item.icon"></i>
      <span slot="title">{{item.label}}</span>
    </el-menu-item>
    <el-submenu index="1" v-for="(item, index) in hasChildren" :key="index">
      <template slot="title">
        <i :class="'el-icon-' + item.icon"></i>
        <span>{{item.label}}</span>
      </template>
      <el-menu-item-group v-for="(butitem, indexs) in item.children" :key="indexs">
        <el-menu-item :index="butitem.path" @click="MenuClick(butitem)">{{ butitem.label }}</el-menu-item>
      </el-menu-item-group>
    </el-submenu>
  </el-menu>
</template>

<script type="text/javascript">
export default {
  data() {
    return {
      asideMenu: [
        {
          path: "/",
          label: "首页",
          name: "home",
          icon: "s-home"
        },
        {
          path: "/video",
          label: "视频管理",
          name: "video",
          icon: "video-play"
        },
        {
          path: "/user",
          label: "用户管理",
          name: "user",
          icon: "user"
        },
        {
          label: "其他",
          icon: "s-operation",
          children: [
            {
              path: "/page1",
              name: "page1",
              label: "页面1"
            },
            {
              path: "/page2",
              name: "page2",
              label: "页面2"
            }
          ]
        }
      ]
    };
  },
  methods: {
    MenuClick(item) {
      this.$router.push({name: item.name});
      this.$store.commit("selectMenu", item);
    }
  },
  computed: {
    // 先使用计算属性过滤data中的数据，然后再渲染可以减少性能消耗
    onChildren() {
      return this.asideMenu.filter(item => !item.children);
    },
    hasChildren() {
      return this.asideMenu.filter(item => item.children);
    },
    isCollpase() {
      return this.$store.state.HanderTab.isCollapse;
    }
  }
};
</script>

<style lang="scss" scoped>
.el-menu {
  height: 100%;
  border: none;
}
.el-menu-vertical-demo:not(.el-menu--collapse) {
    width: 200px;
    min-height: 400px;
  }
</style>
