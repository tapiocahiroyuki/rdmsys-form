<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
    <div clsss="p-3 p-md-0">
      <b-form-rating
        variant="warning"
        size="lg"
        class="rdm-form-stars"
        :id="id"
        :type="type"
        v-model="star"
        :state="hasError"
        :required="required || null"
        @input="updateValue"
        :stars="options.length"
      ></b-form-rating>
    </div>
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

<style lang="scss">
.rdm-form-stars{
  justify-content: center;
  border: 0 !important;
  box-shadow: none;
  > .b-rating-star {
    flex-grow: 0 !important;
  }
  .b-rating-icon{
    flex: 0 1 64px;
    max-width: 64px;
    align-items: center;
    justify-content: center;
    > svg{
      width: 40px;
      height: 40px;
    }
  }
}
</style>