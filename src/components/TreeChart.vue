<template>
  <div class="container" v-if="richMediaData">
    <VueTree
      :dataset="richMediaData"
      :config="treeConfig"
    >
      <template v-slot:node="{ node, collapsed }">
        <div
          class="rich-media-node"
          :style="{ border: collapsed ? '2px solid grey' : '' }">
          <span class="title">Level: {{node.data[0]}}</span>
          <span>ID: {{ node.data[2].tx_id }}</span>
        </div>
      </template>
    </VueTree>
  </div>
</template>

<script>
import VueTree from '@ssthouse/vue-tree-chart'
import data from '../moks/data.json'

export default {
  name: 'treemap',
  components: {
    VueTree
  },
  data() {
    return {
      formattedData: [],
      richMediaData: null,
      treeConfig: { nodeWidth: 120, nodeHeight: 80, levelHeight: 200 }
    }
  },
  created() {
    data.forEach(el => {
      this.formattedData.push({ data: el });
    });
    this.formattedData.forEach(el => {
      this.recurs(el);
    })
  },
  methods: {
    recurs(item) {
      let flag = true;
      this.formattedData.forEach(element => {
        if (element.data[2].vins?.some(el => el.txid === item.data[2].tx_id)) {
          flag = false;
          if (!element.children) {
            element.children = [];
          }
          if (!element.children.some(el => el.data[2].tx_id === item.data[2].tx_id)) {
            element.children.push(item);
          }
        }
      });
      if (flag && !this.richMediaData) {
        this.richMediaData = item;
      }
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.rich-media-node {
  width: 110px;
  display: flex;
  flex-wrap: wrap;
  color: #000;
  background-color: #f9f9f9;
  word-break: break-word;
  line-height: 1.3;
  font-weight: 600;
  box-shadow: 0 3px 5px rgb(0 0 0 / 50%);
  font-size: 12px;
}

.rich-media-node span {
  display: flex;
  padding: 5px;
}
.title {
  width: 100%;
  background: #428bca;
  color: white;
}
.tree-container {
  overflow-y: auto;
  margin: auto;
  width: 100vw;
  height: 100vh;
}
</style>
