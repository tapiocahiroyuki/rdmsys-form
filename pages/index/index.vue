<template>
  <section v-if="section === 'before'" id="form-before">
    <b-alert show variant="secondary">フォームの説明文が表示されます</b-alert>
  </section>
  <section id="form-body" v-else>
    <FmPartsInput
      type="email"
      id="email"
      required="required"
      label="メールアドレス"
      name="email"
      placeholder="mailto@address.com"
      :formValue.sync="formValue.email"
      :errors.sync="errors.email"
      :validate="validate"
      :inReset="inReset"
    />
    <FmPartsInput
      type="text"
      id="name"
      label="お名前"
      name="yourName"
      placeholder="例： 佐藤 和夫"
      :formValue.sync="formValue.name"
      :errors.sync="errors.name"
      :validate="validate"
      :inReset="inReset"
    />
    <FmPartsInput
      type="text"
      id="content"
      label="備考"
      name="content"
      placeholder="例： 宅配ボックスが利用可能です"
      required="required"
      :formValue.sync="formValue.content"
      :errors.sync="errors.content"
      :validate="validate"
      :inReset="inReset"
    />
  </section>
</template>

<script>
export default {
  props: [
    "section",
    "validate",
    "inReset"
  ],
  data() {
    return {
      errors: {},
      formValue: {},
    };
  },
  computed: {
    errorsCount() {
      return this.getErrorsCount();
    },
  },
  methods: {
    updateValue() {
      this.$emit("update:formValue", this.formValue);
      this.formValidation();
    },
    formValidation() {
      this.$emit("update:errors", this.getErrorsCount());
    },
    getErrorsCount() {
      return _.filter(_.values(this.errors),function(x){return _.keys(x).length;}).length;
    },
  },
  watch: {
    validate(){
      if(this.validate) {
        var it = this;
        setTimeout(()=>it.formValidation(),500);
      }
    },
    inReset(){
      this.errors = {};
    },
    formValue(){
      this.updateValue();
    }
  },
};
</script>




