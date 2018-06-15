<template>
  <div class="hello">
    <ul class="list-unstyled mt-3">
      <li class="media my-2" v-for="(post, i) in posts" :key="i">
        <img class="mr-3" :src="post.data.thumbnail" alt="Generic placeholder image">
        <div class="media-body">
          <h5 class="mt-0 mb-1"><a :href="createUrl(post.data.permalink)" target="_blank">{{post.data.title}}</a></h5>
          <div>
            <h3 class="text-danger">{{post.data.ups}} ⬆️ </h3>
            <p>Created about {{formatDate(post.data.created_utc)}} ago by {{post.data.author}}</p>
            <span class="badge badge-pill badge-secondary">{{post.data.num_comments}} comments</span>
            <button
             @click="post.showImage = !post.showImage"
             v-if="isImage(post)"
             class="btn btn-primary">
              {{post.showImage ? 'Hide': 'Show'}} Image
            </button>
            <div v-if="post.showImage">
              <img :src="post.data.url" alt="">
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import {parse, distanceInWords}  from 'date-fns'

export default {
  name: 'Posts',
  data () {
    return {
      posts: []
    }
  },
  mounted(){
    this.load();
  },
  methods:{
    load(){
      const url = 'https://www.reddit.com/r/rarepuppers/.json'
      //fetch the posts
      fetch(url)
      .then(response => response.json())
      .then(result => {
        //add showImage to each child
        result.data.children.forEach(child => {
          child.showImage = false
        })
        this.posts = result.data.children
      })
    },
    formatDate(date){
      return distanceInWords(parse(date * 1000), new Date())
    },
    createUrl(path){
      return `https://www.reddit.com${path}`
    },
    isImage(post){
     return post.data.url.match(/\.(jpg|png|jpeg|bpm)$/)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
