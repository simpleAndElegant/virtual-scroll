<template>
<div class="viewport" ref="viewport" @scroll="handleScroll">
  <div class="scroll-bar" ref="scrollBar"></div>

  <div 
    class="scroll-list"
    :style="{transform:`translate3d(0, ${offset}px, 0)`}"
  >
    <div :vid="item.id" v-for="item in visibleData" :key="item.id" ref="items">
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
    cacheList() {
      this.positions = this.items.map((item, index) => ({
        height: this.size,
        top: index * this.size,
        bottom: (index + 1)  * this.size
      }))
    },
    getStartIndex(value) {
      let start = 0;
      let end = this.positions.length -  1;
      while (start <= end) {
        let middleIndex = Math.ceil((start + end)/2)
        let middleValue = this.positions[middleIndex].bottom;
        let temp = null;
        if(middleValue == value) {
          return middleIndex + 1
        } else if (middleValue < value) { // 目标在右
          start = middleIndex + 1
        }else {
          if (temp === null ||  temp > middleIndex) {
            temp = middleIndex;
          }
          end = middleIndex - 1
        }
        return temp
      }
    },
    handleScroll() {
      // step one 计算滚上去几个了，当前应该从第几个开始展示

      let scrollTop = this.$refs.viewport.scrollTop;
      if (this.variable) {
        // 通过二分法查找对应记录
        this.start = this.getStartIndex(scrollTop)
        this.end = this.start + this.remain;
        const item = this.positions[this.start - this.prevCount]
        this.offset = item.top ?item.top : 0;
      }else  {
        // 滚上去几个了
        this.start = Math.floor(scrollTop / this.size);
        this.end = this.start + this.remain;
      }

    }
  },
  mounted() {
    this.$refs.viewport.style.height =  this.size * this.remain + 'px';
    this.$refs.scrollBar.style.height =  this.items.length + this.size + 'px';
    // 对于不定高度的情况 加载完，保存每一项的高度
    this.cacheList();

  },

  updated()  {
    // 更新缓存
    this.$nextTick(() => {
      let nodes = this.$refs.items;
      if(!(nodes&&nodes.length > 0)) return;
      nodes.forEach((node) => {
        let  { height } = node.getBoundingClientRect()
        let id = node.getAttribute('vid') - 0;
        let oldHeight =  this.positions[id].height;
        let val = oldHeight - height;
        if(val) {  //  缓存高度和真实高度一样
          this.positions[id].height = height;
          this.positions[id].bottom = this.positions[id].bottom - val;
          // 后面都所有人都要改
          for (let i = id + 1; i < this.positions.length; i++){
            this.positions[i].top = this.positions[i-1].bottom;
            this.positions[i].bottom =  this.positions[i].bottom - val;
          }
        }
      });
      this.$refs.scrollBar.style.height = this.positions[this.positions.length - 1].bottom  + 'px'
    })
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
