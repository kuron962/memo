<template>
  <article>
    <nuxt-content :document="page" />
  </article>
</template>

<script>
export default {
  async asyncData ({ $content, params}) {
    const page = await $content('article', params.slug).fetch()
    return {
      page
    }
  }
}
</script>
