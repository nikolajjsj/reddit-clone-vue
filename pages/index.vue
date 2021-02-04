<template>
  <v-col justify="center" align="center">
    <v-card
      v-for="post in posts"
      :key="post.id"
      class="my-5 pt-5 text-left"
      max-width="80%"
    >
      <div class="f-flex px-5">
        <strong> r/{{ post.data.subreddit }} </strong>
        . Posted by {{ post.data.author }} .
        {{ post.data.num_comments }} comments
      </div>

      <v-card-title class="headline2">
        {{ post.data.title }}
      </v-card-title>
      <v-card-text class="text-truncate" v-if="post.data.selftext">
        {{ post.data.selftext }}
      </v-card-text>
      <a :href="post.data.url" target="_blank">
        <v-img
          v-if="isImage(post.data.url)"
          :src="post.data.url"
          max-height="300"
        >
        </v-img>
        <v-btn class="ma-2 text-truncate" v-else elevation="2" >
          {{ post.data.url.split('//')[1].split('/')[0] }}
        </v-btn>
      </a>
    </v-card>
  </v-col>
</template>

<script>
const URL = 'https://www.reddit.com/hot.json';

export default {
  
  data () {
    return {
      title: 'Reddit clone',
    }
  },
  methods: {
    isImage(url) {
      return(url.match(/\.(jpeg|jpg|gif|png)$/) != null);
    }
  },
  async asyncData({  $axios }) {
      const posts = await $axios.$get(URL).then(res => res.data.children);
      return { posts }
    },
}
</script>