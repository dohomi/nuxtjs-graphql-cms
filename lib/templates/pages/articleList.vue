<template>
  <v-container fluid
               grid-list-md class="content-boxed blog-page">
    <lc-article-list :only-blog-posts="true"
                     :is-content-element-visible="true"/>
  </v-container>
</template>
<script>
  // import getHeadMeta from '../util/getHeadMeta'
  import initialAsyncData from '~initialAsyncData'
  import headMetaMixin from '../mixins/headMetaMixin'

  export default {
    layout: 'list',
    mixins: [headMetaMixin],
    head () {
      return this.getHeadMeta({
        article: {},
        languageKey: this.locale,
        overwrites: {
          // overwrites
          description: this.$t('head.meta.articleListDescription'),
          title: this.$t('head.meta.articleListTitle')
        }
      })
    },
    async asyncData ({route, store, req, params, app}) {
      let {host, locale} = initialAsyncData({req, store, params, $cms: app.$cms})
      locale = (app.$cms.routes.listMapLocale && app.$cms.routes.listMapLocale[route.name]) || locale
      await store.dispatch('setLanguageKey', locale)
      return {host, locale}
    }
  }
</script>
