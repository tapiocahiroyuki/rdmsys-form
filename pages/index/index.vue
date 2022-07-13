<template>
  <section v-if="section === 'before'" id="form-before">
    <b-alert show variant="secondary">フォームの説明文が表示されます</b-alert>
  </section>
  <section id="form-body" v-else>
    <b-row class="my-1" tag="dl">
      <b-col md="3" tag="dt">
        <label for="email"
          >メールアドレス<b-badge variant="danger">必須</b-badge></label
        >
      </b-col>
      <b-col md="9" tag="dd">
        <b-form-input
          id="email"
          type="email"
          v-model="formValue"
          :state="hasError"
          placeholder="mailto@address.com"
          required="required"
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
  </section>
</template>

<script>
import isEmail from "is-email";

export default {
  props: [
    "section",
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
      console.log(this.validate);
      if(this.validate) {
        this.formValidation();
      }
    },
    inReset(){
      this.errors = {};
      this.formValue = "";
    }
  },
  methods: {
    updateValue() {
      this.$emit("update:formValue", this.formValue);
      this.formValidation();
    },
    formValidation() {
      var errors = {};
      if (_.isEmpty(this.formValue)) errors.required = 1;
      if (!isEmail(this.formValue)) errors.email = 1;
      this.errors = errors;
      this.$emit("update:errors", this.errors);
    },
  },
};
</script>




