<template>
  <div id="app">
    <VirtualList
      :size="40"
      :remain="8"
      :items="items"
    >
    <Item slot-scope="{item}" :item="item" />
    </VirtualList>
  </div>
</template>

<script>
import VirtualList from './components/Virtual-list.vue'
import Item from './components/Item.vue'
let items = [];
for (let i = 1; i < 1000; i++) {
  items.push({id: i, value: i})
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
