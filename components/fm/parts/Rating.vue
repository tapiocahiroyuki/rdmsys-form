<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
    <b-form-rating
      variant="warning"
      :id="id"
      :type="type"
      v-model="star"
      :state="hasError"
      :required="required || null"
      @input="updateValue"
      :stars="options.length"
    ></b-form-rating>
    <p class="mt-2">Value: {{ formValue }}</p>
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
  mixins: [Mixin],
  computed: {
    star: {
        get(){
            var num = _.invert(this.options)[this.formValue];
            if(!_.isUndefined(num)) num++;
            return num;
        },
        set(newValue){
            this.formValue = this.options[newValue - 1];
            this.$emit("update:formValue", this.formValue);
        }
    },
  },
};
</script>