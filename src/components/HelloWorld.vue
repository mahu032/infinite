<template>
  <div>
    <span v-for="item in tabs" :key="item.id" @click="switchTab(item)" :style="{color:item.id===params?'red':''}">{{item.name}}</span>
    <div class="infinite-wrapper" infinite-wrapper>
      <PullTo @top-pull="toPull" @bottom-pull="toPull" @scroll="scroll" :topLoadMethod="topLoad" :bottomLoadMethod="bottomLoad">
        <div slot="bottom-block">底---部</div>
        <component :is="showModal" :list="list" />
      </PullTo>
      <!-- <component :is="showModal" :list="list" :style="{display:display}" /> -->
      <!-- <infinite-loading :on-infinite="getData" ref="infinite">
        <span slot="no-results" style="display:none;"></span>
        <div slot="no-more" class="tips-nolist">no-more</div>
      </infinite-loading> -->
    </div>
  </div>
</template>

<script>
import InfiniteLoading from './infinite/components/InfiniteLoading'
import PullTo from './PullTo'
import Demo from './Demo'
export default {
  name: 'HelloWorld',
  data() {
    return {
      showModal: 'Demo',
      tabs: [
        { name: 'tab1', id: 0 },
        { name: 'tab2', id: 1 }
      ],
      params: 0,
      offset: 0,
      total: 20,
      list: [],
      display: 'block'
    }
  },
  methods: {
    switchTab(item) {
      this.showModal = Demo
      this.params = item.id
      this.offset = 0
      this.list = []
      // 让列表高度 0
      // this.display = 'none'
      this.$refs.infinite.$emit('$InfiniteLoading:reset')
      // this.$nextTick(() => {
      //   // 等更新完毕再触发reset
      //   this.$refs.infinite.$emit('$InfiniteLoading:reset')
      // })
      console.log(this.$refs.infinite)
    },
    getData(load) {
      // this.display = 'block'
      if (this.list.length) {
        this.offset += 5
      }
      setTimeout(() => {
        let str = this.tabs[this.params].name
        for (var i = 1; i < 6; i++) {
          this.list.push(str + '-----------' + (i + this.offset))
        }
        // this.$refs.infinite.$emit('$InfiniteLoading:loaded')
        if (this.offset === this.total) {
          this.$refs.infinite.$emit('$InfiniteLoading:complete')
        }
      }, 20000)
    },
    toPull(diff) {
      console.log('位移量', diff)
    },
    scroll(event) {
      console.log('滚动事件', event)
    },
    topLoad(load) {
      this.list = []
      this.offset = 0
      this.getData(load)
    },
    bottomLoad(load) {
      this.getData(load)
    }
  },
  components: {
    InfiniteLoading,
    PullTo,
    Demo
  }
}
</script>
<style scoped>
span {
  display: inline-block;
  border: 1px solid #333;
  width: 40vw;
  height: 50px;
  line-height: 50px;
}
.infinite-wrapper {
  height: 60vh;
  overflow-y: auto;
}
</style>
