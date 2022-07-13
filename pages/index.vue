 <template>
  <main>
    <h1 class="text-center">問い合わせフォーム</h1>
    <NuxtChild section="before" />
    <p>プライバシーポリシーはこちら</p>
    <FmProgress :phase="progressPhase"/>
    <form id="form" @submit="onSubmit" method="POST">
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
          <div><b-button variant="danger" type="submit" class="mx-2">この内容で送信する</b-button></div>
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
import Mailgun from "mailgun.js";
import formData from "form-data";
const phase = {
  input: 0,
  validate: 1,
  hasError: 2,
  confirm: 3,
  sendError: 4,
  complete: 5,
};

const mailgun = new Mailgun(formData);

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
    onSubmit(e) {
      e.preventDefault();
      var mg = mailgun.client({
        username: "api",
        key: process.env.MAILGUN_API_KEY,
      });
      const domain = process.env.MAILGUN_DOMAIN;

      var it = this;
      mg.messages
        .create(domain, {
          from: process.env.MAILGUN_FROM_ADDRESS,
          to: [this.formValue],
          subject: "Hello",
          text: "Testing some Mailgun awesomness!",
          html: "<h1>Testing some Mailgun awesomness!</h1>",
        })
        .then((msg) => it.phaseValue = it.phase.complete) 
        .catch((err) => it.phaseValue = it.phase.sendError); 
    },
  }
};
</script>
