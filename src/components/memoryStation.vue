<template>
  <div>
    <div v-if="isOver">
      <div v-show="page>=0" class="w-1/6 ... mx-5 ... float-left ... rounded-3xl ...
   border-solid ...  border-4 ... border-light-blue-500 ...  border-opacity-100 ... ">
        <div class="h-12 flex justify-center items-center text-lg ... ">
          第{{ page % 32 }}页
        </div>
        <hr class="border-2 ..."/>
        <command v-for="item in commands" v-bind:key="item" :command-index=item></command>
      </div>
    </div>
    <div v-else>
      <div v-show="page>=0" class="w-1/6 ... mx-5 ... float-left ... rounded-3xl ...
   border-solid ...  border-8 ... border-red-500 ...  border-opacity-100 ... ">
        <div class="h-12 flex justify-center items-center text-lg ... ">
          第{{ page % 32 }}页
        </div>
        <hr class="border-2 ..."/>
        <command v-for="item in commands" v-bind:key="item" :command-index=item></command>
      </div>
    </div>
  </div>
</template>

<script>

import Command from "@/components/command";

export default {
  name: "memoryStation",
  components: {Command},
  data() {
    return {}
  },
  computed: {
    // 计算属性的 getter
    isOver() {
      // `this` 指向 vm 实例
      return this.page < 32
    }
  },
  props: {
    page: String,
    commands: [Number, Number, Number, Number, Number, Number, Number, Number, Number, Number]
  }
}
</script>

<style scoped>

</style>