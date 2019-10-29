<template>
  <Page v-if="single.type === 'page'" :page="single"/>
  <Post v-else :post="single"/>
</template>

<script>
import Page from '~/components/templates/Page'
import Post from '~/components/templates/Post'

export default {
  async asyncData(context) {
    const { route, app, error } = context

    try {
        // console.log(route.params.single)
      const single = await app.$wp.slug().name(route.params.single)
      return { single }
    } catch (e) {
      error(e)
    }
  },

  components: {
    Page,
    Post
  },

  created(){
      console.log(this.single)
  },

  jsonld() {
      return {
          "@context": "https://schema.org",
          "@type": "NewsArticle",
          "mainEntityOfPage": {
              "@type": "WebPage",
              "@id": "https://google.com/article"
          },
          "headline": this.single.title.rendered,
          "datePublished": this.single.date,
          "description": this.single.content.rendered,
          "author": {
              "@type": "Person",
              "name": "John Doe"
          },
          "publisher": {
              "@type": "Organization",
              "name": "Google",
              "logo": {
                  "@type": "ImageObject",
                  "url": "https://google.com/logo.jpg"
              }
          },
          "image": [
              "https://example.com/photos/1x1/photo.jpg",
              "https://example.com/photos/4x3/photo.jpg",
              "https://example.com/photos/16x9/photo.jpg"
          ],
      };
  },
}
</script>
