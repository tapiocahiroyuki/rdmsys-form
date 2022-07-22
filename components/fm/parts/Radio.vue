<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
    <b-form-group :label="label" v-slot="{ ariaDescribedby }" :id="id">
      <b-form-radio
        v-for="(item, key) in options"
        v-model="formValue"
        :state="hasError"
        :required="required || null"
        :aria-describedby="ariaDescribedby"
        :name="name"
        :value="(item.value) || item"
        :key="key"
        @change="updateValue"
        :disabled="item.disabled" 
        >{{ (item.text) || item }}</b-form-radio
      >
    </b-form-group>
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
  mixins: [Mixin],
};
</script>
