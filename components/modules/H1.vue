<template>
    <component :is="tag || 'h1'" class="rdm-h1 border-primary p-1 p-md-2 fw-600"><slot /></component>
</template>

<script>
export default {
    props: [
        "tag",
    ]
}
</script>

<style lang="scss" scoped>
@import "../../layouts/css/initialization";

.rdm-h1{
    margin-bottom: 4rem;

    border-bottom: 3px solid transparent;

    font-size: 3rem;
    text-align: center;
    @include media-breakpoint-down(sm){
        margin-bottom: 1rem;
        
        font-size: 1.25rem;
    }
}
</style>