<template>
  <header>
    <b-navbar class="header-navbar" toggleable="lg" type="light" variant="primary">
      <b-navbar-brand href="#" class="header-navbar-brand">
        <b-img
          class="header-navbar-brand"
          src="/logos/logo_mark_wh.svg"
          width="451"
          height="96"
          alt="Red Magic Systems"
          title="Red Magic Systems"
          fluid
        />
      </b-navbar-brand>
    </b-navbar>
  </header>
</template>

<style lang="scss" scoped>
@import '../../layouts/css/initialization';

.header-navbar{
  padding: 0;
  box-shadow: rgba(0,0,0,.3) 0 0 16px;
}
.header-navbar-brand {
  display: flex;
  align-items: center;
}
.header-navbar-brand{
  @include media-breakpoint-down(sm){
    width: 225px;
  }
}
</style>