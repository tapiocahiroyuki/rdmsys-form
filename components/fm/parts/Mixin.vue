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
    "inReset",
    "size",
    "options"
  ],
  data() {
    return {
      errors: {},
      formValue: null
    };
  },
  computed: {
    hasError() {
      if (this.isEmptyFormValue() && _.isUndefined(this.errors.required)) return null;
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
      this.setInitialFormValue();
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
      if (this.required && this.isEmptyFormValue()) errors.required = 1;
      if (this.type==="email" && !isEmail(this.formValue||"")) errors.email = 1;
      this.errors = errors;
      this.$emit("update:errors", this.errors);
    },
    isEmptyFormValue(){
      return (_.isArray(this.formValue)) ? (this.formValue.length === 0) : _.isEmpty(this.formValue);
    },
    setInitialFormValue(){
      this.formValue = (_.isArray(this.formValue)) ? [] : null;
    }
  },
};
</script>