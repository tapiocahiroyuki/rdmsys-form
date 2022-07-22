<template>
  <div id="send-mail">
    <slot />
  </div>
</template>

<script>
/*global _ */
import Mailgun from "mailgun.js";
import formData from "form-data";

const mailgun = new Mailgun(formData);

export default {
  props: [
    "data", // 添付ファイル、およびテンプレートに渡すデータを受け取る
    "yourName", // 送信者の名前
    "subject", // メールの題名
    "text", // テンプレートを適用しない場合のテキストメールの内容
    "from", // メールの送信元
    "to", // メールの送信先
    "template", // mailgunのテンプレート
  ],
  computed: {
    attachment() {
      // フォーム項目のうち、ファイル（配列の中にオブジェクトがあるもの）を除いたものを抽出
      var dataFiles = _.flatten(
        _.values(
          _.pickBy(this.data, function (d) {
            return _.isObject(d[0]);
          })
        )
      );
      // {data: file, filename: filename} のコレクションに組み替える
      return _.map(dataFiles, function (v) {
        return { filename: v.name, data: v };
      });
    },
    dataPart() {
      // フォーム項目のうち、ファイル送信以外の項目を抽出
      return _.omitBy(this.data, function (d) {
        return _.isObject(d[0]);
      });
    },
    // 開発ツールでデータ（X-Mailgun-Variables）値を可視化するための算出プロパティ
    sendData() {
      return this.getSendData();
    },
    // mailgun APIに渡す値を生成させる
    sendMailContent() {
      return {
        from: this.from || process.env.MAILGUN_FROM_ADDRESS,
        to: [this.to || process.env.MAILGUN_FROM_ADDRESS],
        subject: this.subject || "Thank You!",
        template: this.template,
        "h:X-Mailgun-Variables": JSON.stringify(this.getSendData()),
        text: this.text,
        attachment: this.attachment,
      };
    },
  },
  methods: {
    // テンプレートに適用する値を生成させる。題名やテキストデータも記載できるようにする
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
  mounted() {
    // メールを送信する動作を司る
    var mg = mailgun.client({
      username: "api",
      key: process.env.MAILGUN_API_KEY,
    });
    const domain = process.env.MAILGUN_DOMAIN;
    var it = this;
    mg.messages
      .create(domain, this.sendMailContent)
      .then((msg) => it.$emit("success", msg))
      .catch((err) => it.$emit("error", err));
  },
};
</script>
