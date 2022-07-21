<template>
  <b-progress class="my-3 border" max="3" height="2rem" striped>
    <b-progress-bar value="1" :variant="(num<=phase)?'danger':'light'" v-for="(label,num) in labels" :key="num">
        <span :class="(num<=phase)?'text-white':'text-dark'">{{label}}</span>
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
