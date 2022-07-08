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
export default {
  name: "IndexPage",
  methods: {
    onSubmit(e) {
      e.preventDefault();
      alert("投稿されました");
    },
  },
};
</script>
