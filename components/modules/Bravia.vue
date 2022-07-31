<template>
  <b-card>
    <b-card-text>
      <div class="d-flex flex-column flex-md-row">
        <div class="flex-grow-0">
          <div class="rdm-bravia-img text-center mb-5 mb-md-0">
            <slot name="image" />
          </div>
        </div>
        <div class="flex-grow-1 ml-0 ml-md-4">
          <h2 class="rdm-bravia-header">{{ header }}</h2>
          <div class="rdm-bravia-lead">{{ lead }}</div>
          <div class="rdm-bravia-slot"><slot /></div>
        </div>
      </div>
    </b-card-text>
  </b-card>
</template>

<script>
export default {
  props: ["header", "lead", "image"],
};
</script>

<style lang="scss" scoped>
@import "../../layouts/css/initialization";

.rdm-bravia-header {
  margin-bottom: 0;

  font-size: 4rem;
  @include font-weight(700);
}

.rdm-bravia-lead {
  margin-bottom: 1.5rem;

  font-size: 1.2rem;
  color: $gray-600;
}
</style>
