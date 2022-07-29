<template>
    <component :is="tag || 'h1'" class="rdm-h1 border-primary p-2 fw-600"><slot /></component>
</template>

<script>
export default {
    props: [
        "tag",
    ]
}
</script>

<style lang="scss" scoped>
.rdm-h1{
    text-align: center;
    margin-bottom: 4rem;
    border-bottom: 3px solid transparent;
    font-size: 3rem;
}
</style>