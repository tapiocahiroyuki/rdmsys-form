<template>
  <section v-if="section === 'before'" id="form-before">
    <b-alert show variant="secondary">フォームの説明文が表示されます</b-alert>
  </section>
  <section id="form-body" v-else>
    <Component
      v-for="(part, key) in formParts"
      :key="key"
      :is="`FmParts${part.is}`"
      :type="email"
      :id="email"
      :required="part.required"
      :label="part.label"
      :name="email"
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
const formParts = [
  {
    is: "Input",
    type: "email",
    id: "email",
    required: "required",
    label: "メールアドレス",
    name: "email",
    placeholder: "mailto@address.com",
  },
  {
    is: "Input",
    type: "text",
    id: "name",
    label: "お名前",
    name: "yourName",
    placeholder: "例： 佐藤 和夫",
  },
  {
    is: "Textarea",
    id: "content",
    label: "備考",
    name: "content",
    placeholder: "例： 宅配ボックスが利用可能です",
    required: "required",
  },
  {
    is: "Radio",
    id: "experience",
    label: "購入経験",
    name: "experience",
    required: "required",
    options: [
      "あり",
      "なし",
      {
        disabled: true,
        value: "不明",
        text: "忘れた",
      },
    ],
  },
  {
    is: "Checkbox",
    id: "check",
    label: "軒先への搬入",
    name: "check",
    options: [
      "馬車が入れます",
      "魔法のじゅうたんが入れます",
      {
        disabled: true,
        value: "不明",
        text: "不明",
      },
    ],
  },
  {
    is: "Rating",
    id: "stars",
    label: "評価",
    name: "stars",
    required: "required",
    options: ["不満", "やや不満", "普通", "満足", "大満足"],
  },
  {
    is: "Select",
    id: "select",
    label: "好きな急行停車駅",
    name: "select",
    required: "required",
    options: [
      {
        value: null,
        text: "-- お選びください --",
      },
      {
        value: "二子玉川",
        text: "二子玉川",
      },
      {
        value: "用賀",
        text: "用賀",
        disabled: true,
      },
      {
        value: "桜新町",
        text: "桜新町",
        disabled: true,
      },
      {
        value: "駒澤大学",
        text: "駒澤大学",
        disabled: true,
      },
      {
        value: "三軒茶屋",
        text: "三軒茶屋",
      },
    ],
  },
  {
    is: "File",
    id: "file",
    label: "ファイル送信",
    name: "file",
    required: "required",
    accept: ".jpg, .png, .gif, .pdf",
  },
];

export default {
  props: ["section", "validate", "inReset"],
  data() {
    return {
      errors: {},
      formValue: {},
      formParts: formParts,
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




