<template>
    <footer>
        <div class="footer-bg bg-primary">
            <b-container class="p-3 mb-3">
            <div class="footer-copy">Red Magic Systems <br class="d-block d-md-none" /> Copyright &copy; 2022, All Rights reserved.<br />
                このサイトのコンテンツを無断で転載することを禁じます。
            </div>
            </b-container>
        </div>
    </footer>
</template>

<style lang="scss" scoped>
@import '../../layouts/css/initialization';

.footer-bg{
    display: flex;
    justify-content: center;
    color: white;
}
.footer-copy{
    @include font-weight(300);
    @include media-breakpoint-down(sm){
    font-size: 12px;
    }
}
</style>