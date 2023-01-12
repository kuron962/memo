<template>
  <article>
    <div v-for="p in pages" :key="p.path">
      <router-link :to="p.path">
        <article-card :page="p"></article-card>
      </router-link>
    </div>
  </article>
</template>

<script>
import ArticleCard from '~/components/ArticleCard.vue';

export default {
  components: { ArticleCard },
  async asyncData ({ $content, route}) {
    const pages = await $content('app', {deep:true}).sortBy('path').fetch()
    return {
      pages
    }
  }
}
</script>
