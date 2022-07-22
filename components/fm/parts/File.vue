<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
    <b-form-file
      :id="id"
      v-model="formValue"
      :state="hasError"
      drop-placeholder="ここにファイルをドロップしてください"
      :placeholder="placeholder || 'ファイルを選択してください'"
      :required="required || null"
      @input="updateValue"
      :accept="accept"
      multiple
    />
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
  props: ["accept"],
  data(){
    return{
      formValue: []
    }
  },
  mixins: [Mixin],
};
</script>
