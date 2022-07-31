<template>
  <b-row class="rdm-form-group mb-0" tag="dl" no-gutters>
    <b-col md="3" tag="dt" class="rdm-form-part-title">
      <label :for="id"><b-badge variant="primary" v-if="required" class="mr-2">必須</b-badge>{{label}}</label>
    </b-col>
    <b-col md="9" tag="dd" class="rdm-form-part-body">
      <slot />
      <b-form-invalid-feedback :id="id">
        <ul class="list-unstyled mb-0">
          <li v-if="errors.email">メールアドレスが不正です</li>
          <li v-if="errors.required">入力必須項目です</li>
        </ul>
      </b-form-invalid-feedback>
    </b-col>
  </b-row>
</template>


<script>
export default {
  props: [
    "label",
    "id",
    "required",
    "errors"
  ]
};
</script>

<style lang="scss">
@import "../../../layouts/css/initialization";

.rdm-form-group {
  .rdm-form-part-title,
  .rdm-form-part-body{
    padding: 1rem;
    margin-bottom: 0;

    color: $gray-800;
    font-size: 1rem;
    @include media-breakpoint-down(sm){
      padding: 2rem 1rem;
    }
  }

  .rdm-form-part-title{
    background-color: $gray-100;

    color: $gray-900;
    font-size: 1.1rem;
    @include media-breakpoint-down(sm){
      padding: .75rem 1rem;
      border-bottom: 1px solid $gray-300;
    }

    > label{
      margin-bottom: .25rem;
      @include media-breakpoint-down(sm){
        margin-bottom: 0;
        
      }
    }
  }

  .form-group{
    margin-bottom: 0;
  }
}
</style>
