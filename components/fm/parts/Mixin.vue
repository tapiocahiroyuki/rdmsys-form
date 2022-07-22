<script>
/*global _ */
// メールフォームのそれぞれの入力項目共通で利用するスクリプト

// テキストがemailアドレスであることを確認するためのライブラリ
import isEmail from "is-email";

export default {
  props: [
    // フォームに設定される属性値
    "type",
    "id",
    "required", 
    "name",
    "placeholder",
    "size",
    // フォームのラベルとして設定される値
    "label",
    // フォームバリデーション中のフラグ
    "validate",
    // 値の初期化フラグ
    "inReset",
    // Select、Radio、Checkboxで利用する選択肢を受け取る
    "options"
  ],
  data() {
    return {
      errors: {}, // バリデーションの結果を保持する
      formValue: null // フォームの値を保持する
    };
  },
  computed: {
    // バリデーションエラーの発生中フラグ
    hasError() {
      if (this.isEmptyFormValue() && _.isUndefined(this.errors.required)) return null;
      return _.isEmpty(_.keys(this.errors));
    },
  },
  watch: {
    validate(){
      // バリデーション中のフラグが立った場合は、
      if(this.validate) {
        // フォームバリデーションを実施する
        this.formValidation();
      }
    },
    // リセットフラグが立った場合
    inReset(){
      // エラーの初期化を行う
      this.errors = {};
      // フォームの値の初期化を行う
      this.setInitialFormValue();
      // 親コンポーネントに値を更新させる
      this.sendValue();
    }
  },
  methods: {
    // フォームのコンポーネントで値の更新イベントを受け取ると発動するメソッド
    updateValue() {
      // 親コンポーネントに値を更新させる
      this.sendValue();
      // フォームバリデーションを実施する
      this.formValidation();
    },
    // 親コンポーネントに値を更新させる
    sendValue(){
      this.$emit("update:formValue", this.formValue);
    },
    // フォームバリデーションを行う
    formValidation() {
      // チェックの結果、エラーが発生した場合のラベルがエラーとして格納される
      this.errors = _.omitBy({
        required: this.checkRequired(),
        email: this.checkEmail()
      },_.isUndefined);
      // 親コンポーネントにエラーを送信する
      this.$emit("update:errors", this.errors);
    },
    // 入力必須項目のチェック
    checkRequired(){
      if(this.required && this.isEmptyFormValue()) return 1;
    },
    // 電子メールアドレスのチェック
    checkEmail(){
      if(this.type==="email" && !isEmail(this.formValue||"")) return 1;
    },
    // フォームの値に入力があるかどうかを判定する
    isEmptyFormValue(){
      // テキストの場合と配列の場合とで判定方法が異なる
      return (_.isArray(this.formValue)) ? (this.formValue.length === 0) : _.isEmpty(this.formValue);
    },
    // フォーム値の初期化
    // コンポーネントで定義した形式によって、空配列、nullのいずれかを格納する
    setInitialFormValue(){
      this.formValue = (_.isArray(this.formValue)) ? [] : null;
    }
  },
};
</script>