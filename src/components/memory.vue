<template>
  <div class="float-left ... w-7/12 ">
    <div class="h-12 flex justify-center items-center text-lg ... ">内存中的页面</div>
    <div>
      <div class="float-left text-white ... ">
        dsadsad
      </div>
      <memory-station v-for="item in commandIndex" :key="item.page" :page=item.page :commands="item.commands">

      </memory-station>
    </div>
  </div>
</template>

<script>
import MemoryStation from "@/components/memoryStation";

export default {
  name: "memory",
  data() {
    return {

    }
  },
  props: {
    commandIndex: Array[{page:String,commands:[Number,Number,Number,Number,Number,Number,Number,Number,Number,Number]}
    , {page:String,commands:[Number,Number,Number,Number,Number,Number,Number,Number,Number,Number]},
      {page:String,commands:[Number,Number,Number,Number,Number,Number,Number,Number,Number,Number]},
      {page:String,commands:[Number,Number,Number,Number,Number,Number,Number,Number,Number,Number]}]
  },
  components: {MemoryStation}
}

</script>

<style scoped>

</style>