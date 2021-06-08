<template>
  <div>
    <br/>
    <div class="flex justify-between bg-blue-200 ...">
      <div class="ml-36 border-r-2 border-fuchsia-600 mt-8 pr-36 ...">
        <div>
          <p class="font-semibold text-xl ... ">作业指令总数</p>
          <p class="font-medium text-base ... ">320</p>
        </div>
        <div>
          <p class="font-semibold text-xl ... ">每页存放指令数</p>
          <p class="font-medium text-base ... ">10</p>
        </div>
        <div>
          <p class="font-semibold text-xl ... ">作业占用内存页数</p>
          <p class="font-medium text-base ... ">4</p>
        </div>
      </div>
      <div class="mt-8 ... ">
        <div>
          <p class="font-semibold text-xl ... ">页面置换算法</p>
          <el-select v-model="algorithmValue" placeholder="请选择">
            <el-option
                v-for="item in algorithmOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
            </el-option>
          </el-select>
        </div>
        <br/>
        <div>
          <p class="font-semibold text-xl ... ">指令执行顺序</p>
          <el-select v-model="commandOrderType" placeholder="请选择">
            <el-option
                v-for="item in commandOrder"
                :key="item.value"
                :label="item.label"
                :value="item.value">
            </el-option>
          </el-select>
        </div>
      </div>
      <div class="mr-48 border-l-2 border-fuchsia-600 mt-8 pl-48 ...">
        <div>
          <p class="font-semibold text-xl ... ">缺页数</p>
          <p class="font-medium text-base ... ">{{ pageMissingNumber }}</p>
        </div>

        <br/>
        <br/>
        <div>
          <p class="font-semibold text-xl ... ">缺页率</p>
          <p class="font-medium text-base ... ">{{ fractional }}%</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "implementInformation",
  props: {
    pageMissingRate: Number,
    pageMissingNumber: Number
  },
  data() {
    return {
      algorithmOptions: [{value: 'FIFO', label: 'FIFO算法'}, {value: 'LRU', label: 'LRU算法'}],
      commandOrder: [{value: 'sequential', label: '顺序执行'},
        {value: 'random', label: '乱序执行'}],
      algorithmValue: 'LRU算法',
      commandOrderType: '乱序执行'
    }
  },
computed:{
  fractional: function () {
    let n=this.pageMissingRate
    //小数保留位数
    let bit = 2;
    //加上小数点后要扩充1位
    bit++;
    //数字转为字符串
    n = n.toString();
    //获取小数点位置3.333333331
    let point = n.indexOf('.');
    //n的长度大于保留位数长度
    if (n.length > point + bit) {
      //保留小数后一位是否大于4，大于4进位
      if (parseInt(n.substring(point + bit, point + bit + 1)) > 4) {
        return n.substring(0, point) + "." + (parseInt(n.substring(point + 1, point + bit)) + 1);
      } else {
        return n.substring(0, point) + n.substring(point, point + bit);
      }
    }
    return n;
  }
},

  methods: {

  }
}
</script>

<style scoped>

</style>