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
  @include media-breakpoint-down(sm){
    margin-bottom: 1rem;
  }
}
.card{
  box-shadow: rgba(0,0,0,.25) 0 6px 12px;
}

.card-body{
  @include media-breakpoint-down(sm){
    padding: 2rem 1rem;
  }
}
.card-title{
  text-align: center;
  font-size: 2rem;
  @include font-weight(700);
  @include media-breakpoint-down(sm){
    font-size: 1.25rem;
  }
}
.form-before-box {
  margin-left: 3rem;
  margin-right: 3rem;
  @include media-breakpoint-down(sm){
    margin-left: 0;
    margin-right: 0;
  }
}

.form-before-p,
.privacy-p  {
  @extend %font-md;
}

.rdm-form-btn{
  font-size: $input-btn-font-size-lg;
  padding: $input-btn-padding-y-lg $input-btn-padding-x-lg;
  @include media-breakpoint-down(sm){
    font-size: $input-btn-font-size;
    padding: $input-btn-padding-y $input-btn-padding-x;
  }
}
</style>


