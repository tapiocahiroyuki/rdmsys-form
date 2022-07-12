 <template>
  <main>
    <h1 class="text-center">問い合わせフォーム</h1>
    <NuxtChild section="before" />
    <p>プライバシーポリシーはこちら</p>
    <form id="form" @submit="onSubmit" method="POST">
      <NuxtChild section="form-body" v-bind:formValue.sync="formValue" v-bind:errors.sync="errors"/>
      <div class="d-flex flex-row align-items-center justify-content-center mb-5">
        <div>
          <b-button variant="light" class="mx-2">リセット</b-button>
        </div>
        <div>
          <b-button variant="primary" class="mx-2">送信確認</b-button>
        </div>
      </div>
      <b-card id="form-confirm" title="この内容でよろしいですか？">
        <b-card-body>
        <b-row tag="dl">
          <b-col tag="dt" md="3">Email</b-col>
          <b-col tag="dd" md="9">{{ formValue }}</b-col>
        </b-row>
        </b-card-body>
      <div class="d-flex flex-row align-items-center justify-content-center">
        <div><b-button variant="light" class="mx-2">入力画面に戻る</b-button></div>
          <div><b-button variant="danger" type="submit" class="mx-2">この内容で送信する</b-button></div>
      </div>
      </b-card>
    </form>
  </main>
</template>

<script>
import Mailgun from "mailgun.js";
import formData from "form-data";
const mailgun = new Mailgun(formData);

export default {
  data(){
    return {
      formValue: "",
      errors: {}
    }
  },
  name: "IndexPage",
  methods: {
    onSubmit(e) {
      e.preventDefault();
      var mg = mailgun.client({
        username: "api",
        key: process.env.MAILGUN_API_KEY,
      });
      const domain = process.env.MAILGUN_DOMAIN;

      mg.messages
        .create(domain, {
          from: process.env.MAILGUN_FROM_ADDRESS,
          to: [this.formValue],
          subject: "Hello",
          text: "Testing some Mailgun awesomness!",
          html: "<h1>Testing some Mailgun awesomness!</h1>",
        })
        .then((msg) => console.log(msg)) // logs response data
        .catch((err) => console.error(err)); // logs any error
    },
  }
};
</script>
