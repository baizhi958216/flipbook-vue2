<template>
  <div id="app">
    <div class="menu">
      <div @click="pageClick(1)">第一页</div>
      <div @click="pageClick(2)">第二页</div>
      <div @click="pageClick(3)">第三页</div>
      <div @click="pageClick(4)">第四页</div>
    </div>
    <Flipbook class="flipbook" :pages="pages" :pagesHiRes="pagesHiRes" :startPage="pageNum" v-slot="flipbook"
      ref="flipbook" @flip-left-start="onFlipLeftStart" @flip-left-end="onFlipLeftEnd"
      @flip-right-start="onFlipRightStart" @flip-right-end="onFlipRightEnd" @zoom-start="onZoomStart"
      @zoom-end="onZoomEnd">
      <div class="action-bar">
        <left-icon class="btn left" :class="{ disabled: !flipbook.canFlipLeft }" @click="flipbook.flipLeft" />
        <plus-icon class="btn plus" :class="{ disabled: !flipbook.canZoomIn }" @click="flipbook.zoomIn" />
        <span class="page-num">
          Page {{ flipbook.page }} of {{ flipbook.numPages }}
        </span>
        <minus-icon class="btn minus" :class="{ disabled: !flipbook.canZoomOut }" @click="flipbook.zoomOut" />
        <right-icon class="btn right" :class="{ disabled: !flipbook.canFlipRight }" @click="flipbook.flipRight" />
      </div>
    </Flipbook>
  </div>
</template>

<script>
import 'vue-material-design-icons/styles.css'
import LeftIcon from 'vue-material-design-icons/ChevronLeftCircle'
import RightIcon from 'vue-material-design-icons/ChevronRightCircle'
import PlusIcon from 'vue-material-design-icons/PlusCircle'
import MinusIcon from 'vue-material-design-icons/MinusCircle'
import Flipbook from 'flipbook-vue/vue2'

export default {
  components: { Flipbook, LeftIcon, RightIcon, PlusIcon, MinusIcon },
  data() {
    return {
      pages: [],
      pagesHiRes: [],
      hasMouse: true,
      pageNum: null,
    }
  },
  methods: {
    onFlipLeftStart(page) { console.log('flip-left-start', page) },
    onFlipLeftEnd(page) {
      console.log('flip-left-end', page)
      window.location.hash = '#' + page
    },
    onFlipRightStart(page) { console.log('flip-right-start', page) },
    onFlipRightEnd(page) {
      console.log('flip-right-end', page)
      window.location.hash = '#' + page
    },
    onZoomStart(zoom) {
      console.log('zoom-start', zoom)
    },
    onZoomEnd(zoom) {
      console.log('zoom-end', zoom)
    },
    setPageFromHash() {
      const n = parseInt(window.location.hash.slice(1), 10)
      if (isFinite(n)) this.pageNum = n
    },
    pageClick(pageNum) {
      this.pageNum = pageNum; // 更新当前页数
    }
  },
  mounted() {
    window.addEventListener('keydown', (ev) => {
      const flipbook = this.$refs.flipbook
      if (!flipbook) return
      if (ev.keyCode == 37 && flipbook.canFlipLeft) flipbook.flipLeft()
      if (ev.keyCode == 39 && flipbook.canFlipRight) flipbook.flipRight()
    })

    // Simulate asynchronous pages initialization
    setTimeout(() => {
      this.pages = [
        null,
        'images/1.jpg',
        'images/2.png',
        'images/3.png',
        'images/1.jpg',
      ]
      this.pagesHiRes = [
        null,
        'images/1.jpg',
        'images/2.png',
        'images/3.png',
        'images/1.jpg',
      ]
    }, 1)

    window.addEventListener('hashchange', this.setPageFromHash)
    this.setPageFromHash()
  },
}
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  align-items: center;
  background-color: #333;
  color: #ccc;
  overflow: hidden;
}

.menu {
  left: 10rem;
  top: 50%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  gap: 10px;
  margin-left: 30px;
  padding: 10px 20px;
  z-index: 999;
  flex-grow: 1;
  height: calc(100vh) !important;
}

.menu div {
  color: white;
  cursor: pointer;
}

a {
  color: inherit;
}

.action-bar {
  width: 100%;
  height: 30px;
  padding: 10px 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.action-bar .btn {
  font-size: 30px;
  color: #999;
}

.action-bar .btn svg {
  bottom: 0;
}

.action-bar .btn:not(:first-child) {
  margin-left: 10px;
}

.has-mouse .action-bar .btn:hover {
  color: #ccc;
  filter: drop-shadow(1px 1px 5px #000);
  cursor: pointer;
}

.action-bar .btn:active {
  filter: none !important;
}

.action-bar .btn.disabled {
  color: #666;
  pointer-events: none;
}

.action-bar .page-num {
  font-size: 12px;
  margin-left: 10px;
}

.flipbook .viewport {
  width: 90vw !important;
  background-color: antiquewhite;
  height: calc(100vh - 50px - 40px) !important;
}

.flipbook .bounding-box {
  box-shadow: 0 0 20px #000;
}

.credit {
  font-size: 12px;
  line-height: 20px;
  margin: 10px;
}
</style>