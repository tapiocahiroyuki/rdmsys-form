<template>
  <b-row class="my-1" tag="dl">
    <b-col md="3" tag="dt">
      <label :for="id">{{label}}<b-badge variant="danger" v-if="required">必須</b-badge>
        </label>
    </b-col>
    <b-col md="9" tag="dd">
      <b-form-input
        :id="id"
        :type="type"
        v-model="formValue"
        :state="hasError"
        :placeholder="placeholder || 'mailto@address.com'"
        :required="required || null"
        @input="updateValue"
      ></b-form-input>
      <b-form-invalid-feedback id="email">
        <ul class="list-unstyled">
          <li v-if="errors.email">メールアドレスが不正です</li>
          <li v-if="errors.required">入力必須項目です</li>
        </ul>
      </b-form-invalid-feedback>
    </b-col>
  </b-row>
</template>


<script>
import isEmail from "is-email";

export default {
  props: [
    "type",
    "id",
    "required",
    "label",
    "name",
    "placeholder",
    "validate",
    "inReset"
  ],
  data() {
    return {
      errors: {},
      formValue: "",
    };
  },
  computed: {
    hasError() {
      if (this.formValue === "" && _.isUndefined(this.errors.required)) return null;
      return _.isEmpty(_.keys(this.errors));
    },
  },
  watch: {
    validate(){
      if(this.validate) {
        this.formValidation();
      }
    },
    inReset(){
      this.errors = {};
      this.formValue = "";
      this.$emit("update:formValue", this.formValue);
    }
  },
  methods: {
    updateValue() {
      this.$emit("update:formValue", this.formValue);
      this.formValidation();
    },
    formValidation() {
      var errors = {};
      if (this.required && _.isEmpty(this.formValue)) errors.required = 1;
      if (this.type==="email" && !isEmail(this.formValue)) errors.email = 1;
      this.errors = errors;
      this.$emit("update:errors", this.errors);
    },
  },
};
</script>
