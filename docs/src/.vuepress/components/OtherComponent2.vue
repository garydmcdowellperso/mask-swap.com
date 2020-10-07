<template>
  <div>
    <img :src="profile.image" /><!-- grab the profile picture -->
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
      this.getMastodonProfile();
    },
    methods: {
      getMastodonProfile() {
        // change this to use your account Id
        fetch('https://simpatico.cloud/api/v1/getMask')
          .then(stream => stream.json())
          .then(data => this.profile = data)
          .catch(error => console.error(error))			
      }
    }
  }
</script>
