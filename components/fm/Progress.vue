<template>
  <b-progress class="rdm-progress my-3 border" max="3" height="2.5rem">
    <b-progress-bar class="rdm-progress-item" value="1" :variant="(num<=phase)?'primary':'light'" v-for="(label,num) in labels" :key="num">
        <span :class="(num<=phase)?'text-white':'text-dark'"><b class="rdm-progress-text fw-500">{{label}}</b></span>
    </b-progress-bar>
  </b-progress>
</template>

<script>
// プログレスバーを表示させる
export default {
    // 処理フェーズの値を受け取る
    props: ['phase'],
    data(){
        return {
            labels: [
                '内容の入力',
                '送信前確認',
                '送信完了'
            ]
        }
    }
}
</script>

<style lang="scss" scoped>
@import "../../layouts/css/initialization";

.rdm-progress-item{
    position: relative;
}
.rdm-progress-text{
    font-size: 1.3rem;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    @include media-breakpoint-down(sm){
        font-size: .85rem;
  }
}

</style>
