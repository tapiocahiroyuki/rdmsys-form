 <template>
  <div id="bs-modules">
    <ul class="modules-list list-unstyled">
      <li class="modules-item mb-5">
        <h2 class="bs-modules-title">buttons</h2>
        <ModulesButtons>
          <template #left>
            <b-button
              variant="outline-dark"
              @click="onResetButtonClicked"
              class="rdm-form-btn"
              >前の画面に戻る</b-button
            >
          </template>
          <template #right>
            <b-button
              variant="primary"
              @click="onResetButtonClicked"
              class="rdm-form-btn"
              >入力画面に戻る</b-button
            >
          </template>
        </ModulesButtons>
      </li>
      <li class="modules-item mb-5">
        <h2 class="bs-modules-title">bravia</h2>
        <ModulesBravia
          header="送信完了"
          lead="お問い合わせありがとうございました。"
          image="/images/send-mail-success.svg"
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
          <p class="mb-5">
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
      </li>
      <li
        v-for="(mod, key) in bsComponents"
        :key="key"
        class="modules-item mb-5"
      >
        <h2 class="bs-modules-title">{{ mod }}</h2>
        <component
          class="mb-1"
          v-for="(b, k) in variables"
          show
          :key="k"
          :bg-variant="b"
          :variant="b"
          :is="'b-' + mod"
        >
          {{ b }}
        </component>
      </li>
    </ul>
  </div>
</template>     

<script>
export default {
  computed: {
    variables() {
      return [
        "primary",
        "secondary",
        "success",
        "info",
        "warning",
        "danger",
        "light",
        "dark",
      ];
    },
    bsComponents() {
      return ["button", "card", "alert"];
    },
  },
  methods: {
    onResetButtonClicked() {
      alert("入力画面に戻るよ");
    },
  },
};
</script>