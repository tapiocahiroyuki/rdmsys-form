<template>
  <div id="send-mail">
    <b-button variant="danger" class="mx-2" @click="sendMail">
      <slot />
    </b-button>
  </div>
</template>

<script>
import Mailgun from "mailgun.js";
import formData from "form-data";

const mailgun = new Mailgun(formData);

export default {
  props: ["data", "yourName", "subject", "text", "from"],
  computed: {
    attachment() {
      // フォーム項目のうち、ファイル（配列の中にオブジェクトがあるもの）を抽出
      var dataFiles = _.flatten(_.values(_.pickBy(this.data, function (d) {
        return _.isObject(d[0]);
      })));
      // {data: file, filename: filename} のコレクションに組み替える
      return _.map(dataFiles,function(v){
        return {'filename':v.name,data:v}; 
      });
    },
    dataPart() {
      // フォーム項目のうち、ファイル送信以外の項目を抽出
      return _.omitBy(this.data, function (d) {
        return _.isObject(d[0]);
      });
    },
  },
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
          to: [this.from],
          subject: this.subject || "Thank You!",
          template: "inquery-form",
          "h:X-Mailgun-Variables": JSON.stringify(this.getSendData()),
          attachment: this.attachment
        })
        .then((msg) => it.$emit("success", msg))
        .catch((err) => it.$emit("error", err));
    },
    getSendData() {
      return {
        subject: this.subject,
        text: this.text,
        yourName: this.yourName,
        data: this.dataPart,
        from: this.from,
      };
    },
  },
};
</script>
