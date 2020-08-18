<template>
  <div class="tabs">
    <el-tag
      :key="tag.name"
      v-for="tag in tags"
      :closable="tag.name !== 'home'"
      :disable-transitions="false"
      @close="handleClose(tag)"
      @click="changMenu(tag)"
      :effect="$route.name === tag.name ? 'dark' : 'plain'"
      size="small"
    >{{tag.label}}</el-tag>
  </div>
</template>

<script type="text/javascript">
import {mapState,mapMutations} from 'vuex'
export default {
  computed: {
    ...mapState({
      tags: state => state.HanderTab.tabsList
    })
  },
  data() {
    return {
      dynamicTags: ["标签一", "标签二", "标签三"],
      inputVisible: false,
      inputValue: ""
    };
  },
  methods: {
    ...mapMutations({
      close: 'closeTab'
    }),
    handleClose(tag) {
      this.close(tag)
    },
    changMenu(item) {
      this.$router.push({name:item.name}),
      this.$store.commit("selectMenu",item)
    }
  }
};
</script>

<style lang="scss" scoped>
.tabs{
  padding: 20px;
  .el-tag{
    margin-right: 10px;
  }
}
</style>
