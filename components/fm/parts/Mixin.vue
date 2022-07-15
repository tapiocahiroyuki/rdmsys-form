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