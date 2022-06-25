<template>
  <div class="searchTre">
    <div class="head-container">
      <el-input v-model="searchKey" placeholder="请输入" clearable size="small" prefix-icon="el-icon-search"
                style="margin-bottom: 20px" />
    </div>
    <div class="head-container">
      <el-tree :data="config.treeData" :props="config.defaultProps" :expand-on-click-node="false"
               :filter-node-method="filterNode" ref="tree" default-expand-all highlight-current
               @node-click="handleNodeClick" />
    </div>
  </div>
</template>

<script>
const defaultConfig = {
  treeData: [],
  /*点击咱开*/
  expandOnClickNode: false,
  /*展开全部*/
  defaultExpandAll: true,
  /*高亮*/
  highlightCurrent: true,
  defaultProps: {
    children: "children",
    label: "label"
  },
}
export default {
  name: "searchTree",
  props: {
    treeConfig: {
      type: Object
    }
  },
  watch: {
    searchKey(val) {
      this.$refs.tree.filter(val)
    }
  },
  data() {
    return {
      searchKey: ''
    }
  },
  computed: {
    config() {
      return {
        ...defaultConfig,
        ...this.treeConfig
      }
    }
  },
  methods: {
    // 筛选节点
    filterNode(value, data) {
      if (!value) return true;
      return data.label.indexOf(value) !== -1;
    },
    handleNodeClick(data) {
      this.$emit('nodeClick', data)
    },
    setCurrentKey(key) {
      this.$refs.tree.setCurrentKey(key)
    }
  }
}
</script>

<style scoped>
</style>
