<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
    <b-form-select
      v-model="formValue"
      :options="options"
      :id="id"
      :state="hasError"
      :required="required || null"
      :name="name"
      @change="updateValue"
    ></b-form-select>
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
  mixins: [Mixin],
};
</script>
