<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
    <b-form-textarea
      :id="id"
      v-model="formValue"
      :placeholder="placeholder || '入力してください'"
      :state="hasError"
      :required="required || null"
      @input="updateValue"
      :size="size || 'md'"
      rows="5"
    ></b-form-textarea>
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
  mixins: [Mixin],
};
</script>
