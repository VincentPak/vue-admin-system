<template>
  <div class="ele-tree-wrapper">
    <Hints>
      <template slot="hintName">Tree树形控件</template>
      <template slot="hintInfo">
        <p>element-tree：基于elementUI的Tree组件，可用于清晰的层级结构展示信息，并能展开或折叠</p>
        <p>地址：访问 <el-link type="success" href="https://element.eleme.cn/#/zh-CN/component/tree" target="_blank">element-tree</el-link></p>
      </template>
    </Hints>
    <el-row :gutter="20">
      <el-col :span="8">
        <el-card shadow="always">
          <div slot="header" class="title">可选择节点</div>
          <div class="content-box">
            <el-tree
              :data="treeData"
              node-key="id"
              show-checkbox
              default-expand-all
              :default-checked-keys="[5]"
              :props="defaultProps"
              @check-change="handleCheckChange">
            </el-tree>
          </div>
        </el-card>
      </el-col>
      <el-col :span="8">
        <el-card shadow="always">
          <div slot="header" class="title">懒加载节点</div>
          <div class="content-box">
            <el-tree
              :props="props"
              :load="loadNode"
              lazy
              show-checkbox
              @check-change="handleCheckChange">
            </el-tree>
          </div>
        </el-card>
      </el-col>
      <el-col :span="8">
        <el-card shadow="always">
          <div slot="header" class="title">可拖拽节点</div>
          <div class="content-box">
            <el-tree
              :data="treeData"
              node-key="id"
              default-expand-all
              :props="defaultProps"
              draggable>
            </el-tree>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Hints from '../../components/Hints/index'
import { treeData } from './data'

export default {
  name: 'EleTree',
  components: { Hints },
  data () {
    return {
      treeData,
      props: {
        label: 'name',
        children: 'zones',
        isLeaf: 'leaf'
      },
      defaultProps: {
        children: 'children',
        label: 'label'
      },
      count: 1
    }
  },
  methods: {
    loadNode (node, resolve) {
      if (node.level === 0) {
        return resolve([{ name: 'region1' }, { name: 'region2' }])
      }
      if (node.level > 2) return resolve([])

      setTimeout(() => {
        let data = [{
          name: 'zone' + this.count++,
          leaf: true
        }, {
          name: 'zone' + this.count++
        }, {
          name: 'zone' + this.count++,
          leaf: true
        }]

        resolve(data)
      }, 500)
    },
    handleCheckChange (data, checked, indeterminate) {
      console.log(data, checked, indeterminate)
    }
  }
}
</script>

<style lang="less">
.ele-tree-wrapper{
  .title{
    text-align: center;
    font-size: 14px;
    font-weight: bold;
  }
  .content-box{
    height: 360px;
  }
}
</style>
