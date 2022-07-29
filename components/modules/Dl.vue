<template>
  <section class="rdm-dl-container">
    <b-row class="rdm-dl" tag="dl" v-for="(value, key) in values" :key="key" no-gutters>
      <b-col class="rdm-dt" tag="dt" md="3"><div class="rdm-dt-cell bg-light">{{ key }}</div></b-col>
      <b-col class="rdm-dd" tag="dd" md="9">
        <div class="rdm-dd-cell">
        <ul v-if="isArray(value)">
          <li v-for="(item, k) in value" :key="k">
            {{ typeof item === "object" ? item.name : item }}
          </li>
        </ul>
        <span v-else>{{ value }}</span>
        </div>
      </b-col>
    </b-row>
  </section>
</template>

<script>
export default {
  props: ["values"],
  methods: {
    // lodashが使えないtemplate節で使う関数
    isArray(value) {
      return _.isArray(value);
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../../layouts/css/initialization';

.rdm-dl{
    margin-bottom: .25rem;
    .rdm-dd, .rdm-dt, ul {
        margin-bottom: 0;
    }
}
.rdm-dt-cell,
.rdm-dd-cell{
    padding: 0.75rem 1rem;
    @extend %font-md;
}
</style>