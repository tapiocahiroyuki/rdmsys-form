<template>
  <!--送信ボタンを表示する。クリックされると管理者側へメールが送信される-->
    <b-button
      v-if="phase === condition.waitForSend"
      variant="danger"
      class="mx-2"
      @click="sendMail"
    >
      <slot />
    </b-button>
    <!--管理者側へメールを送信する。成功したら顧客へメールのカーボンコピーを送信するフェーズへ移行する-->
    <FmMailgun
      v-else-if="phase === condition.sendToOperator && show"
      :from="from"
      @error="onError"
      @success="onSent"
      :subject="yourName + '様よりお問い合わせがありました'"
      :text="dataToString"
      :data="data"
    ><b-button disabled variant="warning"><b-spinner label="管理者側へ送信中" small></b-spinner></b-button>
    </FmMailgun>
    <!--顧客へメールのカーボンコピーを送信する-->
    <FmMailgun
      v-else-if="phase === condition.sendToClient && show"
      :to="from"
      :yourName="yourName"
      @error="onError"
      @success="onSent"
      subject="お問い合わせありがとうございます"
      :data="data"
      template="rdmsys-inquiry"
      ><b-button disabled variant="secondary"><b-spinner label="顧客側へ送信中" small></b-spinner></b-button>
    </FmMailgun>
</template>

<script>
/*global _ */
// メール送信処理フェーズを定義する定数
const phase = {
  waitForSend: 0, // 送信ボタンが表示される状態
  sendToOperator: 1, // 管理者へ送信中
  sendToClient: 2, // 顧客へ送信中
};

export default {
  props: [
    "yourName",
    "from",
    "text",
    "data",
    "subject"
  ],
  data() {
    return {
      condition: phase, // テンプレート節でphase定数を利用するため
      phase: phase.waitForSend, // 処理フェーズを管理する
      msg: [], // APIが返すメッセージを格納するための値
      show: true // 子コンポーネントを点滅させるためのフラグ
    };
  },
  computed: {
    // 運営側へ送信するメッセージ用にテキストメッセージを生成する
    dataToString() {
      var txt = `${this.yourName} 様より、お問い合わせをいただきました。

## 問い合わせの内容
| 項目 | 内容 |
| --- | --- |
`;
      _.forEach(this.data, function (value, key) {
          txt += `| ${key} | ${value} |
`;
      });
      txt += "\n\n";
      return txt;
    },
    phaseText() {
      // フェーズの名前（this.phaseが1ならsendToClient, 0ならsendToOperator）を取得する
      return _.invert(this.condition)[this.phase];
    },
  },
  methods: {
    // メール送信の実行
    sendMail(e) {
      e.preventDefault();
      // フェーズを管理者送信モードに移行する
      this.phase = phase.sendToOperator;
    },
    onSent(msg) {
      // 送信成功メッセージとフェーズを記録する
      this.recordMessage(msg);
      // 次のフェーズへ移行する
      this.phase++;
      // 顧客送信用のmountedフックが起動しなかったので、一旦Mailgunコンポーネントを隠す
      this.flash();
      // 顧客へのメッセージも送信できたら、親コンポーネントに送信完了イベントを発行する
      if (this.phase > phase.sendToClient) this.onSuccess();
    },
    // 一旦Mailgunコンポーネントを隠し、再表示
    flash(){
        this.show = false;
        this.$nextTick(() => {
          this.show = true;
        });
    },
    // 送信エラーが発生した場合の処理を行う
    onError(err) {
      // 送信エラーメッセージとフェーズを記録する
      this.recordMessage(err, true);
      // 親コンポーネントにエラーイベントを発行する
      this.$emit("error", this.msg);
    },
    // メッセージの記録を行う。errorFlagの値がtrueならerrorとして記録する
    recordMessage(msg, errorFlag = false) {
      var log = {};
      // 送信成功したらキーはsuccess。エラーであればキーをerrorに替えてメッセージを記録する
      log[errorFlag ? "error" : "success"] = msg;
      // フェーズの名前を記録する
      log.phase = this.phaseText;
      this.msg.push(log);
      console.log(log);
    },
    // 親コンポーネントに送信完了イベントを発行する
    onSuccess() {
      this.$emit("success", this.msg);
    },
  },
};
</script>