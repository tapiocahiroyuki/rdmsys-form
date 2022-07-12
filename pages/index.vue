 <template>
  <main>
    <h1 class="text-center">問い合わせフォーム</h1>
    <NuxtChild section="before" />
    <p>プライバシーポリシーはこちら</p>
    <form id="form" @submit="onSubmit" method="POST">
      <NuxtChild section="after" />
      <div class="d-flex flex-row align-items-center justify-content-center">
        <div><b-button variant="light">リセット</b-button></div>
        <div>
          <b-button variant="primary" type="submit">送信確認</b-button>
        </div>
      </div>
    </form>
  </main>
</template>

<script>
import Mailgun from "mailgun.js";
import formData from "form-data";
const mailgun = new Mailgun(formData);

export default {
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
          from: "Excited User <mailgun@sandbox-123.mailgun.org>",
          to: [process.env.MAILGUN_FROM_ADDRESS],
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
