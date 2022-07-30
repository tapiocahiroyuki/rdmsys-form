<template>
  <div class="buttons-container">
    <div v-for="item,key in slots" :key="key" class="buttons-cell">
      <slot :name="key"></slot>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    slots(){
      return this.$slots;
    }
  }
}
</script>

<style lang="scss" scoped>
.buttons-container{
  display: flex;
  justify-content: center;
  margin: 0 -10px;
}
.buttons-cell{
  padding: 0 10px;
}

</style>

