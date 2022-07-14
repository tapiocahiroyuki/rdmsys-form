 <template>
  <main>
    <h1 class="text-center">問い合わせフォーム</h1>
    <NuxtChild section="before" />
    <p>プライバシーポリシーはこちら</p>
    <FmProgress :phase="progressPhase"/>
    <form id="form" method="POST">
      <div id="form-inputs" :class="(phaseValue < phase.confirm)?'d-block':'d-none'">
        <b-alert variant="danger" :show="hasError && phaseValue > phase.input">入力エラーがあります</b-alert>
      <NuxtChild section="form-body" v-bind:formValue.sync="formValue" v-bind:errors.sync="errors" :validate="phaseValue === phase.validate" :inReset="inReset" />
      <div class="d-flex flex-row align-items-center justify-content-center mb-5">
        <div>
          <b-button variant="light" type="reset" class="mx-2" @click="onResetButtonClicked">リセット</b-button>
        </div>
        <div>
          <b-button variant="primary" class="mx-2" @click="onConfirmButtonClicked">送信確認</b-button>
        </div>
      </div>
      </div><!-- /#form-inputs -->
      <b-card id="form-confirm" title="この内容でよろしいですか？" v-if="phaseValue === phase.confirm">
        <b-card-body>
        <b-row tag="dl">
          <b-col tag="dt" md="3">Email</b-col>
          <b-col tag="dd" md="9">{{ formValue }}</b-col>
        </b-row>
        </b-card-body>
      <div class="d-flex flex-row align-items-center justify-content-center">
        <div><b-button variant="light" class="mx-2" @click="onBackButtonClicked">入力画面に戻る</b-button></div>
          <FmMailgun :message="formValue" @error="onSendError" @success="onSent" :to="formValue" subject="お問い合わせありがとうございます">この内容で送信する</FmMailgun>
      </div>
      </b-card><!-- /#form-confirm -->
    </form>
    <div>
  <b-jumbotron header="✓送信完了" lead="お問い合わせありがとうございました。" v-if="phaseValue === phase.complete">
    <p>3営業日以内にメールの返信をさせていただきます。しばらくお待ちください。</p>
    <b-button variant="primary" @click="onResetButtonClicked">入力画面に戻る</b-button>
  </b-jumbotron>
  <b-jumbotron header="×送信失敗" lead="メールの送信に失敗しました" v-if="phaseValue === phase.sendError">
    <p>申し訳ありません、メール送信に問題が発生しております。ネットワークの接続も併せてご確認ください。</p>
    <b-button variant="warning" @click="onBackButtonClicked">入力画面に戻る</b-button>
  </b-jumbotron>
</div>
  </main>
</template>

<script>
const phase = {
  input: 0,
  validate: 1,
  hasError: 2,
  confirm: 3,
  sendError: 4,
  complete: 5,
};

export default {
  mixins: [phase],
  data(){
    return {
      formValue: "",
      errors: {},
      phase: phase,
      phaseValue: phase.input,
      inReset: false
    }
  },
  name: "IndexPage",
  computed: {
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
    hasError(){
      return this.getHasError();
    }
  },
  watch: {
    errors(){
      if(this.phaseValue === this.phase.validate) {
        this.phaseValue = (this.getHasError()) ? this.phase.hasError :this.phase.confirm;
      }
    }
  },
  methods: {
    onConfirmButtonClicked(e){
      this.phaseValue = this.phase.validate;
    },
    getHasError(){
      return (!_.isEmpty(_.keys(this.errors)));
    },
    onBackButtonClicked(e){
      this.phaseValue = this.phase.input;
    },
    onResetButtonClicked(e){
      this.phaseValue = this.phase.input;
      this.formValue = "";
      this.errors = {};
      this.inReset = true;
      var it = this;
      this.$nextTick(()=> it.inReset = false);
    },
    onSent(msg){
      console.log({success:msg});
      this.phaseValue = this.phase.complete;
    },
    onSendError(err){
      console.log({error:err});
      this.phaseValue = this.phase.sendError;
    }
  }
};
</script>
