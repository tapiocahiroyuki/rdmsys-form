 <template>
  <main>
    <ModulesH1>お問い合わせフォーム</ModulesH1>
    <NuxtChild />
    <div class="privacy mb-4">
      <p class="privacy-p text-right">
        <NuxtLink to="#">プライバシーポリシー</NuxtLink>はこちら
      </p>
    </div>
    <FmProgress :phase="progressPhase" />
    <form id="form" method="POST" class="rdm-form">
      <div
        id="form-inputs"
        :class="phaseValue < phase.confirm ? 'd-block' : 'd-none'"
      >
        <b-alert
          id="errorMsg"
          variant="danger"
          :show="errors && phaseValue > phase.input"
          >入力エラーがあります</b-alert
        >
        <FmBody
          :formParts="formParts"
          v-bind:formValue.sync="formValue"
          v-bind:errors.sync="errors"
          :validate="phaseValue === phase.validate"
          :inReset="inReset"
        />
        <ModulesButtons>
          <template #left>
            <b-button
              variant="outline-dark"
              type="reset"
              class="rdm-form-btn"
              @click="onResetButtonClicked"
              size="lg"
              >リセット</b-button
            >
          </template>
          <template #right>
            <b-button
              variant="info"
              class="rdm-form-btn text-left"
              @click="onConfirmButtonClicked"
              size="lg"
              >送信確認</b-button
            >
          </template>
        </ModulesButtons>
      </div>
      <!-- /#form-inputs -->
      <div
        id="form-confirm"
        class="rdm-form-confirm"
        v-if="phaseValue === phase.confirm"
      >
        <b-card title="この内容でよろしいですか？">
          <b-card-text class="mb-5">
            <ModulesDl :values="formValueData" />
          </b-card-text>
          <ModulesButtons>
            <template #left>
              <b-button
                variant="outline-dark"
                size="lg"
                class="rdm-form-btn"
                @click="onBackButtonClicked"
                >入力画面に戻る</b-button
              >
            </template>
            <template #right>
              <FmSend
                :yourName="formValue.name"
                @error="onSendError"
                @success="onSent"
                :from="formValue.email"
                subject="お問い合わせありがとうございます"
                :text="formValue.content"
                :data="formValueData"
                >この内容で送信する</FmSend
              >
            </template>
          </ModulesButtons>
        </b-card>
      </div>
      <!-- /#form-confirm -->
    </form>
    <div>
      <ModulesBravia
        header="送信完了"
        lead="お問い合わせありがとうございました。"
        v-if="phaseValue === phase.complete"
        image="https://placehold.jp/200x200.png"
      >
        <template v-slot:image>
          <b-img
            src="/images/send-mail-success.svg"
            title=""
            alt=""
            width="200"
            height="200"
          />
        </template>
        <p>
          3営業日以内にメールの返信をさせていただきます。しばらくお待ちください。
        </p>
        <div class="text-center text-md-left">
          <b-button
            variant="primary"
            @click="onResetButtonClicked"
            size="lg"
            class="rdm-form-btn"
            >入力画面に戻る</b-button
          >
        </div>
      </ModulesBravia>
      <ModulesBravia
        header="送信失敗"
        lead="メールの送信に失敗しました"
        v-if="phaseValue === phase.sendError"
        image="https://placehold.jp/200x200.png"
      >
        <template v-slot:image>
          <b-img
            src="/images/send-mail-fail.svg"
            title=""
            alt=""
            width="200"
            height="200"
          />
        </template>
        <p>
          申し訳ありません、メール送信に問題が発生しております。ネットワークの接続も併せてご確認ください。
        </p>
        <div class="text-center text-md-left">
          <b-button
            variant="warning"
            @click="onBackButtonClicked"
            size="lg"
            class="rdm-form-btn"
            >入力画面に戻る</b-button
          >
        </div>
      </ModulesBravia>
    </div>
  </main>
</template>

<script>
/*global _ */
// フォームの入力項目の構成を記載したJSONファイルを読み込む
import formParts from "../pages/index/index.json";

// 入力から送信までのフェーズを示す定数
const phase = {
  input: 0, // フォーム入力中
  validate: 1, // 全体のフォームバリデーション実行中
  hasError: 2, // 入力エラーが見つかり、エラーメッセージが表示されている状態
  confirm: 3, // 送信前確認の画面表示中
  sendError: 4, // 送信した結果エラーが発生した状態
  complete: 5, // 送信に成功した状態
};

export default {
  data() {
    return {
      // フォームに入力された値を管理する
      formValue: {},
      // エラーが発生している入力項目を管理する
      errors: 0,
      // フォームの入力項目の構成を記載したJSONファイルが読み込まれる
      formParts: formParts,
      // template節でphase定数を扱うのに必要
      phase: phase,
      // 入力フェーズを管理する
      phaseValue: phase.input,
      // リセットボタンが押されるとtrueとなり、それを検知した子コンポーネントはプロパティを初期化する
      inReset: false,
    };
  },
  name: "IndexPage",
  computed: {
    // フェーズによってプログレスバーの赤い部分を塗る場所を示す
    progressPhase() {
      switch (this.phaseValue) {
        case phase.input:
        case phase.validate:
        case phase.hasError:
          return 0;
        case phase.confirm:
        case phase.sendError:
          return 1;
        case phase.complete:
          return 2;
        default:
      }
    },
    // フォーム入力項目に対して、フォームのname属性を引くための配列を生成する
    formPartsName() {
      return this.getFormPartsNames();
    },
    // フォームの名前と入力値の組み合わせの配列を生成する
    formValueData() {
      var obj = {};
      var lookup = this.getFormPartsNames();
      _.forEach(this.formValue, function (value, key) {
        obj[lookup[key]] = value;
      });
      return obj;
    },
  },
  watch: {
    errors() {
      // フォーム全体のバリデーションを行い、errorsの値がnullから別の値に更新された場合
      if (this.phaseValue === this.phase.validate && !_.isNull(this.errors)) {
        if (this.errors != 0) {
          // エラーがある場合
          this.phaseValue = this.phase.hasError;
          // スクロールを「入力エラーがあります」の部分に移動する
          this.scrollToErrorMsg();
        } else {
          // エラーがない場合
          this.phaseValue = this.phase.confirm;
        }
      }
    },
  },
  methods: {
    // 「送信確認」ボタンを押下した時の処理
    onConfirmButtonClicked(e) {
      // エラーをnullに設定することで、エラーの数にかかわらず$watchで検知されるようにする
      this.errors = null;
      // フォーム全体のバリデーションフェーズに移行する
      this.phaseValue = this.phase.validate;
    },
    // 「入力に戻る」ボタンを押下した場合の処理
    onBackButtonClicked(e) {
      // 入力フェーズに戻す
      this.phaseValue = this.phase.input;
    },
    // リセットボタンを押下した場合の処理
    onResetButtonClicked(e) {
      // フォームの初期化処理を行う
      this.formInitialise();
    },
    // 送信成功した場合の処理
    onSent(msg) {
      console.log({ success: msg });
      // 送信完了フェーズに移行し、送信成功画面を表示させる
      this.phaseValue = this.phase.complete;
    },
    // 送信に失敗した場合の処理
    onSendError(err) {
      console.log({ error: err });
      // 送信エラーフェーズに移行し、エラー表示を行う
      this.phaseValue = this.phase.sendError;
    },
    // フォームの初期化処理
    formInitialise() {
      // フォームの値を初期化する
      this.phaseValue = this.phase.input;
      this.formValue = {};
      this.errors = 0;

      // フラグをtrueに設定し、子コンポーネントの初期化を行わせる
      this.inReset = true;
      var it = this;
      // 画面が更新されたら、子コンポーネントの初期化中フラグをfalseに戻す
      this.$nextTick(() => (it.inReset = false));
    },
    // lodashが使えないtemplate節で使う関数
    isArray(value) {
      return _.isArray(value);
    },
    // フォームのパーツのid属性からname属性を引くための辞書を配列で用意する
    getFormPartsNames() {
      var obj = {};
      _.forEach(this.formParts, function (value) {
        obj[value.id] = value.name;
      });
      return obj;
    },
    // エラーが発生した場合、スクロールをフォームのトップに戻す処理を行う
    scrollToErrorMsg() {
      document.location.href = "#form";
    },
  },
  mounted() {
    // 読み込みがかかった場合、子コンポーネントの初期化をさせる
    this.formInitialise();
  },
};
</script>