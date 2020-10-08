<style scoped>
.action-button {
  display: inline-block;
  font-size: 1.2rem;
  color: #fff;
  background-color: #3eaf7c;
  padding: 0.8rem 1.6rem;
  border-radius: 4px;
  transition: background-color 0.1s ease;
  box-sizing: border-box;
  border-bottom: 1px solid #389d70;
}
.hero {
  text-align: center;
}
.uploading-image {
  border-style: solid;
}
.large {
  font-size: 1.3rem;
  font-weight: 600;
}
</style>
<template>
  <div class="hero" v-if="profile.image">
    <p class="large">
      {{profile.name}} @ {{profile.location}}
    </p>
    <p>
      <img :src="profile.image" /><!-- grab the profile picture -->
    </p>
    <p class="large">
      {{profile.timestamp}}
    </p>
    <p>
      <a class="nav-link action-button" @click="getMask()">
        Another →
      </a>
    </p>
  </div>
  <div class="hero" v-else>
    <p class="large">
      Nothing to see here, come back soon or contribute a mask yourself
    </p>
    <p>
      <a class="nav-link action-button" href="/submit">
        Submit →
      </a>
    </p>
  </div>

</template>
<script>
  import "isomorphic-fetch" // keep IE happy
  export default {
    data() {
      return {
        profile: {}
      };
    }, 
    mounted() {
      this.getMask();
    },
    methods: {
      getMask() {
        // change this to use your account Id
        fetch('https://simpatico.cloud/api/v1/getMask')
          .then(stream => stream.json())
          .then(data => this.profile = data)
          .catch(error => console.error(error))			
      }
    }
  }
</script>
