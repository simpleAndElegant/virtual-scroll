<template>
<div class="viewport" ref="viewport" @scroll="handleScroll">
  <div class="scroll-bar" ref="scrollBar"></div>

  <div 
    class="scroll-list"
    :style="{transform:`translate3d(0, ${offset}px, 0)`}"
  >
    <div v-for="item in visibleData" :key="item.id">
        <slot :item="item"></slot>
    </div>
  </div>

</div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: ["size", "remain", 'items'],
  data()  {
    return {
      start: 0,
      end: this.remain,
    }
  },

  methods: {
    handleScroll() {
      // step one 计算滚上去几个了，当前应该从第几个开始展示

      let scrollTop = this.$refs.viewport.scrollTop;
      // 滚上去几个了
      this.start = Math.floor(scrollTop / this.size);
      this.end = this.start + this.remain;


    }
  },
  mounted() {
    this.$refs.viewport.style.height =  this.size * this.remain + 'px';

    this.$refs.scrollBar.style.height =  this.items.length + this.size + 'px';

  },

  computed: {
    visibleData() {
      let start = this.start - this.prevCount;
      let end = this.end + this.nextCount;

      return this.items.slice(start, end)
    },
    offset() {
      return this.start *  this.size - this.size * this.prevCount
    },
    prevCount()  {
      return Math.min(this.start, this.remain)
    },
    nextCount() {
      return Math.min(this.remain, this.items.length - this.end)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.viewport {
  overflow-y: scroll;
  position: relative;
}
.scroll-bar {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
}
</style>
