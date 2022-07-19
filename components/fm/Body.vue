<template>
  <section id="form-body">
    <b-spinner label="Spinning" v-if="!formParts"/>
    <Component v-else 
      v-for="(part, key) in formParts"
      :key="key"
      :is="`FmParts${part.is}`"
      :type="part.type"
      :id="part.id"
      :required="part.required"
      :label="part.label"
      :name="part.name"
      :placeholder="part.placeholder"
      :formValue.sync="formValue[part.id]"
      :errors.sync="errors[part.id]"
      :validate="validate"
      :inReset="inReset"
      :options="part.options"
      :accept="part.accept"
    />
  </section>
</template>

<script>
export default {
  props: ["section", "validate", "inReset", "formParts"],
  data() {
    return {
      errors: {},
      formValue: {}
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
      return _.filter(_.values(this.errors), function (x) {
        return _.keys(x).length;
      }).length;
    },
  },
  watch: {
    validate() {
      if (this.validate) {
        var it = this;
        setTimeout(() => it.formValidation(), 500);
      }
    },
    inReset() {
      this.errors = {};
    },
    formValue() {
      this.updateValue();
    },
  },
};
</script>




