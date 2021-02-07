<template>
  <div id="app">
    <VirtualList
      :size="100"
      :remain="8"
      :items="items"
      :variable="true"
    >
    <Item slot-scope="{item}" :item="item" />
    </VirtualList>
  </div>
</template>

<script>
import VirtualList from './components/Virtual-list.vue'
import Item from './components/Item.vue'
import Mock from 'mockjs'

let items = [];
for (let i = 0; i < 1000; i++) {
  items.push({id: i, value: Mock.Random.sentence(5, 50)})
}


export default {
  name: 'App',
  components: {
    VirtualList,
    Item
  },
  data()  {
    return {
      items
    }
  },
  
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
