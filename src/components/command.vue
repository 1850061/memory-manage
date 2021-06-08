<template>
  <div>
    <div v-if="isOverTen">
      <center>
        <div
            class="w-3/4 ... text-center ... text-white ... my-2 ...  rounded-3xl ... bg-blue-400 ... h-8 ... flex justify-center items-center ... ">
          {{ commandIndex }}
        </div>
      </center>
    </div>
    <div v-else>
      <center>
        <div
            class="w-3/4 ... text-center ... text-white ... my-2 ...  rounded-3xl ... bg-red-400 ... h-8 ... flex justify-center items-center ... ">
          {{ commandIndex % 10 }}
        </div>
      </center>
    </div>
  </div>
</template>

<script>
export default {
  name: "command",
  computed: {
    // 计算属性的 getter
    isOverTen() {
      // `this` 指向 vm 实例
      return this.commandIndex < 10
    }
  },
  props: {
    commandIndex: String
  }
}
</script>

<style scoped>

</style>