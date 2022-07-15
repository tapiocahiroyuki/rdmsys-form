<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
      <b-form-input
        :id="id"
        :type="type"
        v-model="formValue"
        :state="hasError"
        :placeholder="placeholder || 'mailto@address.com'"
        :required="required || null"
        @input="updateValue"
      ></b-form-input>
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
  mixins: [Mixin]
};
</script>
