<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
    <b-form-group v-slot="{ ariaDescribedby }">
      <b-form-checkbox-group
        :id="`checkbox-${id}`"
        v-model="formValue"
        :aria-describedby="ariaDescribedby"
        @change="updateValue"
        :state="hasError"
        :name="name"
        :required="required || null"
      >
        <b-form-checkbox
        v-for="(item, key) in options"
        :value="(item.value) || item"
        :key="key"
        :disabled="item.disabled"
        >{{ (item.text) || item }}</b-form-checkbox
      >
      </b-form-checkbox-group>
    </b-form-group>
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
    data(){
        return {
            formValue: []
        }
    },
  mixins: [Mixin],
    inReset(){
      this.errors = {};
      this.formValue = [];
      this.$emit("update:formValue", this.formValue);
    }
};
</script>
