<template>
  <div>
    <span v-for="item in tabs" :key="item.id" @click="switchTab(item)" :style="{color:item.id===params?'red':''}">{{item.name}}</span>
    <div class="infinite-wrapper" infinite-wrapper>
      <PullTo @top-pull="toPull" @bottom-pull="toPull" @scroll="scroll" :topLoadMethod="topLoad" :bottomLoadMethod="bottomLoad">
        <!--
          slot 支持
          <div slot="no-result"></div>
          <div slot="no-more"></div>

          prop:
          topLoadMethod 下拉刷新方法
          bottomLoadMethod 上拉加载方法
          loadingConfig {
            type: 默认 图案--spinner 文字--text 默认 spinner
            style: default 图案风格   default,bubbles,circles,spiral,waveDots
          }

          topConfig 下拉时提示文字默认值
          {
            pullText: '下拉刷新',
            // 下拉释放提示文字
            triggerText: '释放更新',
            // 下拉加载时文字
            loadingText: '加载中...',
            // 成功文字
            doneText: '加载完成',
            // 失败文字
            failText: '加载失败',
            // 加载完毕等待时间
            loadedStayTime: 400,
            // 默认距离和高度一样
            stayDistance: 50,
            // 高度差多少触发
            triggerDistance: 70
          }
          bottomConfig 上拉时提示文字
          {
            pullText: '上拉加载',
            // 上拉释放提示文字
            triggerText: '释放更新',
            // 上拉加载时文字
            loadingText: '加载中...',
            // 成功文字
            doneText: '加载完成',
            // 失败文字
            failText: '加载失败',
            // 没有更多
            'no-moreText': '没有更多',
            // 没有数据
            'no-resultText': '没有数据',
            // 加载完毕等待时间
            loadedStayTime: 400,
            // 默认距离和高度一样
            stayDistance: 50,
            // 高度差多少触发
            triggerDistance: 70
          }

          topLoadMethod  bottomLoadMethod 调用时 参数 callback 为load
          请求结束请 load(state)

          state: done 请求完成还有更多数据 fail 请求失败 no-more 请求完成没有更多 no-result 请求成功没有数据 reset 重新请求
         -->
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
import PullToRefresh from './PullToRefresh'
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
    PullToRefresh,
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
