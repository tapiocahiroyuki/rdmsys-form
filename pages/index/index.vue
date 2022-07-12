<template>
  <section v-if="section === 'before'" id="form-before">
    <b-alert show variant="secondary">フォームの説明文が表示されます</b-alert>
  </section>
  <section id="form-body" v-else>
    <b-row class="my-1" tag="dl">
      <b-col md="3" tag="dt">
        <label for="email">メールアドレス<b-badge variant="danger">必須</b-badge></label>
      </b-col>
      <b-col md="9" tag="dd">
        <b-form-input
          id="email"
          type="email"
          v-model="formValue"
          :state="false"
          placeholder="mailto@address.com"
          required="required"
          @input="updateValue"
        ></b-form-input>
        <b-form-invalid-feedback id="email">
          <ul class="list-unstyled">
            <li>メールアドレスが不正です</li>
            <li>入力必須項目です</li>
          </ul>
        </b-form-invalid-feedback>
      </b-col>
    </b-row>
  </section>
</template>

<script>
export default {
props: ["section"],
  data() {
    return {
      errors: {},
      formValue: ""
    };
  },
  methods:{
    updateValue(){
      this.$emit('update:formValue', this.formValue);
    }
  }
};
</script>




