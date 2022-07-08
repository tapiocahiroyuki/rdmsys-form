<template>
  <b-alert variant="danger" show>
    <section v-if="section === 'before'" id="form-before">
      <p>フォームの説明文が表示されます</p>
    </section>
    <section id="form-body" v-else>
      <p>フォーム本体が表示されます</p>
    </section>
  </b-alert>
</template>

<script>
export default {
  props: ["section"],
};
</script>


