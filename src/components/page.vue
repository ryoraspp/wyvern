<template>
  <div :class="[$route.meta.type]" v-if="post && post.id">
    <h1 v-html="getHighlighted(post.title.rendered)"></h1>
    <div v-html="post.content.rendered"></div>
  </div>
  <div class="page" v-else-if="$route.meta.content">
    <div v-html="$route.meta.content"></div>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex';

  export default {
    data() {
      return {
      };
    },
    methods: {
      fetchData() {
        this.$store.cacheDispatch('getQuery', {
          post_type: this.$route.meta.type,
          id: this.$route.meta.id,
        });
      },
      getHighlighted(input) {
        return input.replace(this.term, `<span style="background-color:yellow">${this.term}</span>`);
      },
    },
    computed: {
      ...mapGetters({
        term: 'getTerm',
      }),
      post() {
        return this.$store.getters.getQueryById(this.$route.meta.id);
      },
    },
    created() {
      this.fetchData();
      this.title();
    },
    watch: {
      $route: ['fetchData', 'title'],
    },
  };
</script>

<style>

</style>
