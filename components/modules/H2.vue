<template>
  <div class="rdm-h2-container">
    <component :is="tag || 'h2'" class="rdm-h2 border-bottom py-3"
      ><i class="rdm-h2-bullet"></i><slot
    /></component>
  </div>
</template>

<script>
export default {
  props: ["tag"],
};
</script>

<style lang="scss" scoped>
@import "../../layouts/css/initialization";

.rdm-h2 {
  font-size: 2rem;
  @include font-weight(600);

  text-indent: -1em;
  padding-left: 1em;
}

.rdm-h2-bullet::before {
  content: ".";
  color: transparent;
  display: inline-block;
  border-left: 10px solid $primary;
  margin-right: calc(1em - 10px);
}
</style>