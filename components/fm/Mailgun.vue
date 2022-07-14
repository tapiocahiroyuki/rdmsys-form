<template>
  <div id="send-mail">
    <b-button variant="danger" class="mx-2" @click="sendMail">
        <slot />
    </b-button>
  </div>
</template>

<script>
import Mailgun from 'mailgun.js';
import formData from 'form-data';

const mailgun = new Mailgun(formData);

export default {
  props: [
    'message',
    'to',
    'subject'
],
  methods: {
    sendMail(e) {
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
            to: [this.to],
            subject: this.subject || 'Thank You!',
            template: 'inquiry-rdmsys',
            'h:X-Mailgun-Variables': JSON.stringify({email:this.message})
        })
        .then((msg) => it.$emit("success", msg))
        .catch((err) => it.$emit("error", err));
    },
  },
};
</script>
