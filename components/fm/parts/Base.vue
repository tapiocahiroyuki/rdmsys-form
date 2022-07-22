<template>
  <b-row class="my-1" tag="dl">
    <b-col md="3" tag="dt">
      <label :for="id">{{label}}<b-badge variant="danger" v-if="required">必須</b-badge>
        </label>
    </b-col>
    <b-col md="9" tag="dd">
      <slot />
      <b-form-invalid-feedback :id="id">
        <ul class="list-unstyled">
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
