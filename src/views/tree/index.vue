<template>
  <div class="app-container">
    <el-input v-model="filterText" placeholder="Filter keyword" style="margin-bottom:30px;" />

    <el-tree
      ref="tree2"
      :data="data2"
      :props="defaultProps"
      :filter-node-method="filterNode"
      class="filter-tree"
      default-expand-all
    />
    <div>Array = ['elment1', 'elment2', 'vue']</div>
    <el-button type="primary" @click="btnNotFoundMethod">Not Found Method</el-button>
    <el-button type="danger" @click="btnMethodNameTypo">Typo In Method Name</el-button>
    <el-button plain title="Array has no any()" @click="btnSyntxError">Syntx Error</el-button>

  </div>
</template>

<script>
export default {

  data() {
    return {
      filterText: '',
      data2: [{
        id: 1,
        label: 'Level one 1',
        children: [{
          id: 4,
          label: 'Level two 1-1',
          children: [{
            id: 9,
            label: 'Level three 1-1-1'
          }, {
            id: 10,
            label: 'Level three 1-1-2'
          }]
        }]
      }, {
        id: 2,
        label: 'Level one 2',
        children: [{
          id: 5,
          label: 'Level two 2-1'
        }, {
          id: 6,
          label: 'Level two 2-2'
        }]
      }, {
        id: 3,
        label: 'Level one 3',
        children: [{
          id: 7,
          label: 'Level two 3-1'
        }, {
          id: 8,
          label: 'Level two 3-2'
        }]
      }],
      defaultProps: {
        children: 'children',
        label: 'label'
      }
    }
  },
  watch: {
    filterText(val) {
      this.$refs.tree2.filter(val)
    }
  },

  methods: {
    filterNode(value, data) {
      if (!value) return true
      return data.label.indexOf(value) !== -1
    },
    myMethod(arr = []) {
      if (arr.some(d => d === 'vue')) {
        console.log('This array at least has one element named "vue"')
        this.$message('This array at least has one element named "vue"')
      }
    },

    btnMethodNameTypo() {
      this.myMethodd(['elment1', 'elment2', 'vue'])
    },
    btnNotFoundMethod() {
      this.processArrayData()
    },
    btnSyntxError() {
      const arr = ['elment1', 'elment2', 'vue']
      arr.forEach((ele, idx) => {
        console.log(ele)
      })
      if (arr.any(d => d === 'vue')) {
        console.log('Array does not have any() method!')
        this.$message('This array at least has one element named "vue"')
        // this.$notify({
        //   type: 'info',
        //   message: `ele-${idx}: ${ele}`,
        //   duration: 3000
        // })
      }
      this.$message('this message is not displayed due to syntx error in the last line')
    }
  }
}
</script>

