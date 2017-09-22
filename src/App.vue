<template>
  <div id="app">
    <el-table :data="planList" border style="width: 100%" :row-style="showTr" 
                  :highlight-current-row="true"  >
            <el-table-column label="计划名称" header-align="center" prop="projectPlanName"
                             width="400" :show-overflow-tooltip="true" align="left" >
                <template scope="scope">
                    <span v-for="(space, levelIndex) in scope.row.treeLevel-1" class="ms-tree-space"></span>
                    <span v-if="toggleIconShow(scope.row)" @click="toggle(scope.$index)">
                            <i v-if="!scope.row.treeExpanded" class="el-tree-node__expand-icon" aria-hidden="true"></i>
                            <i v-if="scope.row.treeExpanded" class="el-tree-node__expand-icon expanded" aria-hidden="true"></i>
                    </span>
                    {{ scope.row.projectPlanName }}
                </template>
            </el-table-column>

            <el-table-column label="计划编码" header-align="center" prop="projectPlanCode"
                             width="240" :show-overflow-tooltip="true" align="center" >
            </el-table-column>

            <el-table-column label="说明" header-align="center" prop="remark"
                             :show-overflow-tooltip="true" align="center" >
            </el-table-column>
        </el-table>
  </div>
</template>

<script>
import { getData }  from './dataTree';
export default {
  name: 'app',
  data() {
      return {
          planList:getData()
      }
  },
  methods: {
      // 显示行
            showTr: function (row, index) {
                let parentIndex = this.planList.findIndex(value => {
                    return value.id === row.pid;
                });
                let show = (row.pid ? (this.planList[parentIndex].treeExpanded && this.planList[parentIndex]._show) : true)
                row._show = show
                return show ? '' : 'display:none;'
            },

            // 展开下级
            toggle: function (trIndex) {
                let record = this.planList[trIndex]
                record.treeExpanded = !record.treeExpanded
            },

            // 显示层级关系的空格和图标
            spaceIconShow (index) {
                if (index === 0) {
                    return true
                }
                return false
            },

            // 点击展开和关闭的时候，图标的切换
            toggleIconShow (record) {
                if (record.children && record.children.length > 0) {
                    return true
                }
                return false
            }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.ms-tree-space{
  position: relative;
  top: 1px;
  display: inline-block;
  font-family: 'Glyphicons Halflings';
  font-style: normal;
  font-weight: 400;
  line-height: 1;
  width: 18px;
  height: 14px;
}
.ms-tree-space::before{content: ""}
</style>
