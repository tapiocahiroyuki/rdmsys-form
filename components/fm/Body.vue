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
/*global _ */
export default {
  props: [
    "validate", // フォーム全体のバリデーション中フラグ
    "inReset", // リセットボタンを押した状態を示すフラグ
    "formParts" // フォームを構成する部品データ
  ],
  data() {
    return {
      errors: {}, // エラーの状態を保持する
      formValue: {} // フォームの値を保持する
    };
  },
  computed: {
    // エラーが発生している個数を返す
    errorsCount() {
      return this.getErrorsCount();
    },
  },
  methods: {
    // フォームの内容が更新された時の動作
    updateValue() {
      // 親コンポーネントにフォームの値を更新させる
      this.$emit("update:formValue", this.formValue);
      // エラーの数を更新
      this.updateError();
    },
    // 親コンポーネントに、エラーの数を更新させる
    updateError() {
      this.$emit("update:errors", this.getErrorsCount());
    },
    // エラーの個数を算出する
    getErrorsCount() {
      return _.filter(_.values(this.errors), function (x) {
        return _.keys(x).length;
      }).length;
    },
  },
  watch: {
    validate() {
      // フォーム全体のバリデーションを開始した場合
      if (this.validate) {
        var it = this;
        // すべてのフォームのバリデーションが完了（500ms待つ）したら、親コンポーネントにエラーを伝える。
        setTimeout(() => it.updateError(), 500);
      }
    },
    // リセットボタンを押した場合の処理
    inReset() {
      // エラーの状態を初期化
      this.errors = {};
    },
    // フォームの値が更新された場合の処理
    formValue() {
      // 親コンポーネントにフォームの値を反映させる
      this.updateValue();
    },
  },
};
</script>




