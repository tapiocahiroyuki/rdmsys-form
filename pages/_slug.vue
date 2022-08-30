<template>
  <main>
    <h1>{{slug||'Home'}}</h1>
    <Tutorial />
    <nuxt-link to="/">home</nuxt-link>
    <nuxt-link to="/あいう">あいう</nuxt-link>
    <nuxt-link to="/eo">eo</nuxt-link>
    <nuxt-link to="/かきく">かきく</nuxt-link>
    <nuxt-link to="/keko">keko</nuxt-link>
  </main>
</template>

<script>
import NuxtLogo from '../components/NuxtLogo.vue'
export default {
  components: { NuxtLogo },
  name: 'IndexPage',
  async asyncData({ params }) {
    const slug = params.slug // "/abc" パスにアクセスすると、slug は "abc" になります。
    return { slug }
  }
}
</script>
