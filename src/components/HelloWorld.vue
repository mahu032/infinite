<template>
  <div>
    <span v-for="item in tabs" :key="item.id" @click="switchTab(item)" :style="{color:item.id===params?'red':''}">{{item.name}}</span>
    <component :is="showModal" :list="list"/>
    <infinite-loading :on-infinite="getData" ref="infinite">
      <span slot="no-results" style="display:none;"></span>
      <div slot="no-more" class="tips-nolist">no-more</div>
    </infinite-loading>
  </div>
</template>

<script>
import InfiniteLoading from 'vue-infinite-loading'
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
      list: []
    }
  },
  methods: {
    switchTab(item) {
      this.showModal = Demo
      this.params = item.id
      this.offset = 0
      this.list = []
      this.$refs.infinite.$emit('$InfiniteLoading:reset')
      console.log(this.$refs.infinite)
    },
    getData() {
      if (this.list.length) {
        this.offset += 5
      }
      setTimeout(() => {
        let str = this.tabs[this.params].name
        for (var i = 1; i < 6; i++) {
          this.list.push(str + '-----------' + (i+this.offset))
        }
        this.$refs.infinite.$emit('$InfiniteLoading:loaded')
        if (this.offset === this.total) {
          this.$refs.infinite.$emit('$InfiniteLoading:complete')
        }
      }, 2000)
    }
  },
  components: {
    InfiniteLoading,
    Demo
  }
}
</script>
<style scoped>
span{
  display: inline-block;
  border: 1px solid #333;
  width: 40vw;
  height: 50px;
  line-height: 50px;
}
</style>
