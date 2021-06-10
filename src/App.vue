<template>
  <div id="app" class=" h-screen ...">
    <div class=" ...">
      <my-information></my-information>
    </div>
    <div class="flex items-stretch mb-8 ...">
      <memory :command-index=pages></memory>
      <implement ref="implement"></implement>
    </div>
    <div class="flex justify-center mb-8 ...">
      <a-button class="mx-16 ... " @click="step(false)">单步执行</a-button>
      <a-button class="mx-16 ... " @click="multStep">连续执行</a-button>
      <a-button class="mx-16 ... " @click="reset">重置</a-button>
    </div>
    <implement-information :pageMissingNumber=pageMissingNumber
                           :pageMissingRate="pageMissingRate" ref="information">
    </implement-information>
  </div>
</template>


<script>
import Memory from "@/components/memory";
import Implement from "@/components/implement";
import MyInformation from "@/components/myInformation";
import ImplementInformation from "@/components/implementInformation";


export default {
  name: 'App',
  data() {
    return {
      pages: [{page: -1, commands: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]},
        {page: -1, commands: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]},
        {page: -1, commands: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]},
        {page: -1, commands: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]}],
      pageInformation: [{inTime: 0, uesTime: 0}, {inTime: 0, uesTime: 0}, {inTime: 0, uesTime: 0}, {
        inTime: 0,
        uesTime: 0
      }],
      pageMissingRate: 0,
      pageMissingNumber: 0,
      stepCount: 0,
      nowAddress: -1,
      mulTimeInterval: 400,
      lastClick: 0,
      isMultStep: 0,
      timer: null
    }
  },
  components: {
    ImplementInformation,
    MyInformation,
    Implement,
    Memory,
  },
  methods: {
    step: function (fromMul = false) {
      if (this.isMultStep === 1 && fromMul === false && this.stepCount < 320) {
        alert('正在连续执行，不能按该按钮')
        return
      }
      if (this.stepCount >= 320) {
        alert("指令已执行完毕")
        this.sleep(this.mulTimeInterval)
        return;
      }
      if (this.lastClick === 0) {
        this.lastClick = new Date().getTime()
      } else if ((new Date().getTime() - this.lastClick) < 50) {
        return
      } else {
        this.lastClick = new Date().getTime()
      }
      this.decideNext()
      let memoryIndex = this.isInMemory()
      if (memoryIndex >= 0) {
        this.pageInformation[memoryIndex]['useTime'] = this.stepCount
        this.$refs.implement.addData(this.getData('否', ' -', ' -'))
        this.pageMissingRate = this.pageMissingNumber * 100 / this.stepCount
        this.commandAnimate(memoryIndex, this.nowAddress % 10)
        return
      }
      let zero = this.haveZero()
      if (zero >= 0) {
        this.pages[zero]['page'] = Math.floor(this.nowAddress / 10)
        this.pageInformation[zero]['inTime'] = this.stepCount
        this.pageInformation[zero]['useTime'] = this.stepCount
        this.$refs.implement.addData(this.getData('是', ' -', Math.floor(this.nowAddress / 10)))
        this.pageMissingNumber += 1
        this.pageMissingRate = this.pageMissingNumber * 100 / this.stepCount
        this.pageAnimate(zero)
        this.commandAnimate(zero, this.nowAddress % 10)
        return
      }
      if (this.$refs.information.algorithmValue === 'LRU算法' || this.$refs.information.algorithmValue === 'LRU') {
        let outPageIndex = this.LRU()
        let oldPage = this.pages[outPageIndex]['page']
        this.pages[outPageIndex]['page'] = Math.floor(this.nowAddress / 10)
        this.pageInformation[outPageIndex]['inTime'] = this.stepCount
        this.pageInformation[outPageIndex]['useTime'] = this.stepCount
        this.$refs.implement.addData(this.getData('是', oldPage, Math.floor(this.nowAddress / 10)))
        this.pageMissingNumber += 1
        this.pageMissingRate = this.pageMissingNumber * 100 / this.stepCount
        this.pageAnimate(outPageIndex)
        this.commandAnimate(outPageIndex, this.nowAddress % 10)
      } else if (this.$refs.information.algorithmValue === 'FIFO算法' || this.$refs.information.algorithmValue === 'FIFO') {
        let outPageIndex = this.FIFO()
        let oldPage = this.pages[outPageIndex]['page']
        this.pages[outPageIndex]['page'] = Math.floor(this.nowAddress / 10)
        this.pageInformation[outPageIndex]['inTime'] = this.stepCount
        this.pageInformation[outPageIndex]['useTime'] = this.stepCount
        this.$refs.implement.addData(this.getData('是', oldPage, Math.floor(this.nowAddress / 10)))
        this.pageMissingNumber += 1
        this.pageMissingRate = this.pageMissingNumber * 100 / this.stepCount
        this.pageAnimate(outPageIndex)
        this.commandAnimate(outPageIndex, this.nowAddress % 10)
      }

    },
    multStep: function () {
      if (this.stepCount >= 320) {
        alert("指令已执行完毕")
        return;
      }
      if (this.isMultStep === 1) {
        this.isMultStep = 0
        clearInterval(this.timer)
        return
      }
      const that = this
      this.isMultStep = 1
      that.timer = setInterval(function () {
        that.step(true)
        if (that.stepCount >= 320) {
          clearInterval(that.timer)
          this.isMultStep = 0
          alert("指令已执行完毕")
        }
      }, that.mulTimeInterval);

    },
    isInMemory: function () {
      let page = Math.floor(this.nowAddress / 10)
      for (let i = 0; i < this.pages.length; i++) {
        if (this.pages[i]['page'] % 32 === page) {
          return i
        }
      }
      return -1
    },
    reset: function () {
      if (this.isMultStep === 1 && this.stepCount < 320) {
        alert('正在连续执行，不能按该按钮')
        return
      }
      this.$refs.implement.reset()
      this.iniPages()
      this.pageInformation = [{inTime: 0, uesTime: 0}, {inTime: 0, uesTime: 0}, {inTime: 0, uesTime: 0}, {
        inTime: 0,
        uesTime: 0
      }]
      this.pageMissingRate = 0
      this.pageMissingNumber = 0
      this.stepCount = 0
      this.nowAddress = -1
      this.lastClick = 0
      this.isMultStep = 0
      alert("重置成功")
    },
    FIFO: function () {
      let minIndex = -1;
      let minInTime = 1000
      for (let i = 0; i < this.pageInformation.length; i++) {
        if (this.pageInformation[i]['inTime'] === 0) {
          continue
        }
        if (this.pageInformation[i]['inTime'] < minInTime) {
          minInTime = this.pageInformation[i]['inTime']
          minIndex = i
        }
      }
      return minIndex
    },
    LRU: function () {
      let minIndex = -1;
      let minUseTime = 1000
      for (let i = 0; i < this.pageInformation.length; i++) {
        if (this.pageInformation[i]['useTime'] === 0) {
          continue
        }
        if (this.pageInformation[i]['useTime'] < minUseTime) {
          minUseTime = this.pageInformation[i]['useTime']
          minIndex = i
        }
      }
      return minIndex
    },
    getData: function (loss, outPage, inPage) {
      return {
        index: this.stepCount, place: this.nowAddress, page: Math.floor(this.nowAddress / 10)
        , loss: loss, outPage: outPage, inPage: inPage
      }
    },
    haveZero: function () {
      for (let i = 0; i < this.pages.length; i++) {
        if (this.pages[i]['page'] === -1) {
          return i
        }
      }
      return -1
    },
    sleep: function (time) {
      let startTime = new Date().getTime() + parseInt(time, 10);
      let a = 0
      while (new Date().getTime() < startTime) {
        a = (a + 1) % 10
      }
    }
    ,
    iniPages: function () {
      for (let i = 0; i < this.pages.length; i++) {
        this.pages[i]['page'] = -1;
        this.pages[i]['commands'] = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
      }
    }
    ,
    decideNext: function () {
      console.log(this.$refs.information.commandOrderType)
      if (this.$refs.information.commandOrderType === '乱序执行' || this.$refs.information.commandOrderType === 'random') {
        if (this.nowAddress === -1) {
          this.nowAddress = Math.floor((Math.random() * 320));
        } else {
          if (this.stepCount % 4 === 0 || this.stepCount % 4 === 2) {
            this.nowAddress = (this.nowAddress + 1) % 320
          } else if (this.stepCount % 4 === 1) {
            this.nowAddress = (Math.floor((Math.random() * this.nowAddress))) % 320;
          } else if (this.stepCount % 4 === 3) {
            this.nowAddress = (Math.floor((Math.random() * (319 - this.nowAddress)) + this.nowAddress + 1)) % 320;
          }
        }
      } else {
        this.nowAddress = (this.nowAddress + 1) % 320
      }
      this.stepCount += 1
    }
    ,
    commandAnimate(page, command) {
      const that = this
      that.pages[page]['commands'][command % 10] += 10
      setTimeout(function () {
        that.pages[page]['commands'][command % 10] -= 10
      }, that.mulTimeInterval * 0.8);
    }
    ,
    pageAnimate(page) {
      const that = this
      that.pages[page]['page'] += 32
      setTimeout(function () {
        that.pages[page]['page'] -= 32
      }, that.mulTimeInterval * 0.8);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
