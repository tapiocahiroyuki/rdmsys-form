<template>
  <div class="rdm-h2-container">
    <component :is="tag || 'h2'" class="rdm-h2 border-bottom"
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
  padding: 1rem 0 1rem 1em;

  text-indent: -1em;
  font-size: 2rem;
  @include font-weight(700);
  @include media-breakpoint-down(sm){
    padding: .25rem 0 .25rem 1em;

    font-size: 1rem;
  }
}

.rdm-h2-bullet::before {
  content: ".";
  color: transparent;
  display: inline-block;
  border-left: 10px solid $primary;
  margin-right: calc(1em - 10px);
}
</style>