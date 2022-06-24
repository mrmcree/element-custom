<template>
  <div class="el-table-ruoyi">
    <!--    v-loading="tableConfigDefault.loading"-->
    <el-table
      :default-sort="tableConfigDefault.defaultSort"
      :data="tableList"
      ref="table"
      :height="tableConfigDefault.height"
      @selection-change="handleSelectionChange"
      :fit="tableConfigDefault.fit"
      :border="tableConfigDefault.border"
      :stripe="tableConfigDefault.stripe"
    >
      <el-table-column type="selection" width="50" align="center" v-if="tableConfigDefault.showSelection" />
      <el-table-column type="index" width="50" v-if="tableConfigDefault.showIndex" label="序号" />
      <el-table-column v-for="(tableColumn,index) in this.column"
                       :key="index"
                       :label="tableColumn.label"
                       :prop="tableColumn.prop"
                       :show-overflow-tooltip="tableConfigDefault.showOverflowTooltip"
                       :width="tableColumn.width"
                       :resizable="tableColumn.resizable"
                       :formatter="tableColumn.format || defaultFormat"
      >
        <template v-slot="scope" :scope="tableColumn.slotName" v-if="tableColumn.slotName">
          <slot :name="tableColumn.prop" v-bind="scope">

          </slot>
        </template>
        <template v-slot="scope"  v-else>

            {{ scope.row[tableColumn.prop]  | emptyFormatter }}

        </template>
      </el-table-column>
      <el-table-column
        v-if="actionConfigDefault.operations.length"
        :fixed="actionConfigDefault.fixed || 'right'"
        :label="actionConfigDefault.label || '操作'"
        :width="actionConfigDefault.width || 180"
      >


        <template v-slot="scope" v-if="actionConfigDefault.operations.length">

          <el-button
            v-for="item in actionConfigDefault.operations"
            :key="item.operationType"
            v-hasPermi="[`${item.permissionCode}`]"
            :type="item.type || 'default'"
            :size="item.size || 'mini'"
            :icon="operationIcon(item)"
            @click="item.handleClick(scope.row, item)"
          >{{ item.actionBtnName }}
          </el-button
          >

          <slot name="operation" v-bind="scope"></slot>
        </template>
      </el-table-column>
    </el-table>
    <pagination
      v-show="paginationDefault.total"
      :total="paginationDefault.total"
      :page.sync="paginationDefault.pageNum"
      :limit.sync="paginationDefault.pageSize"
      @pagination="getList"
    />
  </div>

</template>

<script>

export default {
  name : "ElTableRouyi",
  props: {
    pagination: {
      type: Object,
    },

    actionConfig: {
      default: Object
    },
    tableConfig : {
      type: Object
    },
    column      : {
      type   : Array,
      default: [],
    },
    tableList   : {
      type   : Array,
      default: [],
      require: true
    },

  },
  data() {
    return {
      operationTypeIcon: {
        delete: 'el-icon-delete',
        add   : 'el-icon-delete',
        edit  : 'el-icon-edit',
      },
      /**
       * @description 多选项
       */
      mutiSelections: [],
      /**
       * @description 是否已多选
       */
      isMultiple: false,
      /**
       * @description 是否已单选
       */
      isSingle: false
    }

  },
  computed: {
    paginationDefault() {
      return {
        total   : 1,
        pageNum : 1,
        pageSize: 10,
        ...this.pagination,

      }
    },
    tableConfigDefault() {
      return {
        // defaultSort        : { prop: 'createTime', order: 'descending' },
        height             : '60vh',
        fit                : true,
        stripe             : true,
        border             : true,
        showOverflowTooltip: true,
        loading            : false,
        selectionKey       : 'id',
        showIndex          : true,
        showSelection      : true,


        ...this.tableConfig,
      }

    },
    actionConfigDefault() {
      return {
        actionColumnFixed: {
          type   : Boolean,
          default: true,
        },
        operations       : [


        ],
        ...this.actionConfig,

      }

    }
  },
  methods : {
    defaultFormat(row, column, cellValue, index) {
      return cellValue
    },
    handleSelectionChange(selection) {
      this.mutiSelections = selection.map(item => item[this.tableConfigDefault.selectionKey])
      this.multiple = !selection.length;
      this.single = selection.length !== 1;
      this.$emit('selectChange', {
        single        : this.single,
        multiple      : this.multiple,
        mutiSelections: this.mutiSelections
      })
    },
    clearSelection() {
      this.$nextTick(() => {
        if ( !this.$refs.hasOwnProperty('table') ) return
        this.$refs.table.clearSelection()
      })
    },
    operationIcon(item) {
      if ( this.operationTypeIcon.hasOwnProperty(item.operationType) ) {
        return this.operationTypeIcon[item.operationType]
      } else {
        return item.icon
      }
    },
    getList() {
      this.$emit('pagination', this.pagination)
    }
  },
  filters : {
    emptyFormatter(value) {
      if ( typeof value === 'number' ) {
        return value;
      }
      if ( value == null || value === '' ) {
        return '-';
      }
      return value;
    },

  }
}
</script>

<style scoped lang="scss">
.el-table-ruoyi {

}
</style>
