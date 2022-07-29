<template>
  <div id="page" class="d-flex flex-column vh-100">
    <div class="header-container bg-primary">
      <RdmsysHeader />
    </div>
    <b-container class="mb-6 flex-grow-1">
      <Nuxt />
    </b-container>
      <RdmsysFooter/>
  </div>
</template>

<style lang="scss">
@import "./css/custom.scss";

.header-container {
  margin-bottom: 3rem;
}
.card{
  box-shadow: rgba(0,0,0,.25) 0 6px 12px;
}
.card-title{
  text-align: center;
  font-size: 2rem;
  @include font-weight(700);
}
</style>


