<template>
<div>
  <v-layout column justify-center align-center>
    <nuxt-page-logo />
  </v-layout>
  <div v-for="tag in tags" :key="tag">
    <h2 class="headline">{{tag}}</h2>
    <v-container fluid grid-list-xl>
      <v-layout row wrap>
        <v-flex xs12 md3 v-for="post in posts" v-if="post.tags.indexOf(tag) > -1" :key="post.title">
          <v-card class="my-3" hover v-on:click.capture="go(post.path)">
            <v-card-media class="white--text" height="150px" :src="'/images/'+post.image.feature" v-if="post.image.feature">
            </v-card-media>
            <v-card-title>{{post.title}}</v-card-title>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</div>
</template>

<script>
  import NuxtPageLogo from '~/components/PageLogo.vue'
  var _ = require('underscore')

  export default {
    components: {
      NuxtPageLogo
    },
    asyncData: async ({
      app,
      route
    }) => ({
      posts: await app.$content('/news').query({ exclude: 'body' }).getAll()
    }),
    data: function () {
      return {
        tags: []
      }
    },
    created: function () {
      var t = []
      for (var i = 0; i < this.posts.length; i++) {
        for (var n = 0; n < this.posts[i].tags.length; n++) {
          t.push(this.posts[i].tags[n])
        }
      }
      this.tags = _.uniq(t)
    },
    methods: {
      go: function (url) {
        this.$nuxt.$router.push(url)
      }
    },
    filters: {}
  }
</script>
