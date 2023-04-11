<template>
  <article>
    <router-link to="/"> 一覧へ </router-link>
    <nuxt-content :document="page" />
  </article>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const page = await $content("article", params.slug).fetch();
    return {
      page,
    };
  },
};
</script>
