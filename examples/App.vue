<template>
  <div id="app">
    <cascader :props="props" :options="options" size="small" collapse-tags filterable clearable style="width: 400px"/>


<!--    <cascader-panel-->
<!--        :options="options"-->
<!--        :props="props"-->
<!--        style="margin: 200px 0;width: 400px"-->
<!--    />-->

    <cascader
        v-model="lazyValue"
        :props="lazyProps"
        :options="lazyOptions"
        filterable
        style="margin: 100px 0;width: 400px"
        @expand-change="expandChange"
    />
    <cascader
        v-model="lazyCheck"
        :props="lazyMutiProps"
        :options="options"
        filterable
        collapse-tags
        style="margin: 100px 0;width: 400px"
        @expand-change="expandChange"
    />

    <el-button @click="clear">clear</el-button>
<!--    <button @onclick="clear" >clear</button>-->
  </div>
</template>

<script>
import Mock from 'mockjs'
import LazyCascader from 'elp-cascader/packages'


const {
  Cascader,
  // CascaderPanel
} = LazyCascader

let id = 0

export default {
  name: 'APP',
  components: {
    Cascader,

    // CascaderPanel,
  },
  data() {
    console.log('Mock Start')
    console.time('MockStart')
    const _mock = Mock.mock({
      'array|20': [
        {
          label: '@csentence(6)',
          value: '@string()',
          'children|2': [
            {
              label: '@csentence(6)',
              value: '@string()',
              'children|2': [
                {
                  label: '@csentence(6)',
                  value: '@string()'
                }
              ]
            }
          ]
        }
      ]
    })
    const _singleMock = Mock.mock({
      'array|1000': [
        {
          label: '@csentence(6)',
          value: '@string()'
        }
      ]
    })
    console.warn(_singleMock)
    console.timeEnd('MockStart')
    return {
      value: [],
      // options: [],
      options: _mock.array,
      props: {
        multiple: false,
        infiniteScroll: true,
        checkStrictly: false,
        selectWithExpand: false,
        panelSearch: true,
        panelLabels: ['一级类目', '二级类目', '三级类目'],
        lazy: true,
        lazyLoad(node, resolve) {

          const {level} = node
          setTimeout(() => {
            const nodes = Array.from({length: level + 11}).map(() => ({
              value: String(++id),
              label: `选项${id}`,
              // disabled: true,
              leaf: level >= 3
            }))
            // resolve(nodes)
            // 通过调用resolve将子节点数据返回，通知组件数据加载完成
            resolve({list: nodes, isEnd: node.pageNo === 2})
          }, 1000)
        }
        // expandTrigger: 'hover'
      },
      // lazy cascader
      lazyValue: [
        ['1', '2']
      ],
      lazyCheck: [],
      lazyOptions: [
        // { label: '111', value: '111', children: [] },
        // { label: '选项1', value: '1', children: [{ label: '选项2', value: '2' }] }
      ],
      lazyProps: {
        // lazy: true,
        infiniteScroll: true,
        multiple: true,
        checkStrictly: false,
        lazyMultiCheck: true,
        panelLabels: ['一级类目', '二级类目', '三级类目'],
        lazyLoad(node, resolve) {
          const {level} = node
          setTimeout(() => {
            const nodes = Array.from({length: level + 1}).map(() => ({
              value: String(++id),
              label: `选项${id}`,
              leaf: level >= 3
            }))
            // 通过调用resolve将子节点数据返回，通知组件数据加载完成
            resolve(nodes)
          }, 1000)
        }
      },
      lazyMutiProps: {
        lazy: true,
        infiniteScroll: true,
        multiple: true,
        // checkStrictly: true,
        // panelSearch: true,
        // checkAll: true,
        // panelLabels: ['数据连接', '分类', '数据源'],
        lazyLoad(node, resolve) {
          console.log('aaaaaa', node)
          const {level} = node
          setTimeout(() => {
            const nodes = Array.from({length: level + 11}).map(() => ({
              value: String(++id),
              label: `选项${id}`,
              // disabled: true,
              leaf: level >= 1
            }))
            // resolve(nodes)
            // 通过调用resolve将子节点数据返回，通知组件数据加载完成
            resolve({list: nodes, isEnd: node.pageNo === 2})
          }, 200)
        }
      }
    }
  },
  methods: {
    expandChange(parentArr) {
      console.info(parentArr)
    },
    clear() {
      console.log('aasdfasdfasdf')
      this.lazyCheck = [];
    }
  }
}
</script>

<style>
#app {
  display: grid;
  place-items: flex-start;
  margin: 60px;
}
</style>

