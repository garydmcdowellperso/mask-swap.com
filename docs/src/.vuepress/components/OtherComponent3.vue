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
</style>
<template>
  <div class="hero">
    <p v-if="previewImage && !uploaded">
      <img :src="previewImage" class="uploading-image" />
    </p>
    <p v-if="!uploaded">
      <label>Name: </label><input type="text" id="name" v-model="name" placeholder="Your name"/>
    </p>
    <p v-if="!uploaded">
      <label>Where are you?: </label><input type="text" id="location" v-model="location" placeholder="Your location"/>
    </p>
    <p v-if="!uploaded">
      <input type="file" accept="image/*" @change="uploadImage($event)" id="file-input">
    </p>
    <p v-if="previewImage && !uploaded">
      <a class="nav-link action-button" @click="sendImage($event)">
        Submit my mask →
      </a>
    </p>
    <p v-if="uploaded">
      Uploaded
    </p>
    <p v-if="uploaded">
      <a class="nav-link action-button" href="/">
        Home →
      </a>
    </p>
  </div>
</template>
<script>
  import "isomorphic-fetch" // keep IE happy
  export default {
    data() {
      return {
        previewImage: null,
        name: "",
        location: "",
        uploaded: false
      };
    }, 
    methods: {
      sendImage(event) {
        const data = {
          image: this.previewImage,
          name: this.name,
          location: this.location
        };

        const URL = 'https://simpatico.cloud/api/v1/putMask'; 

        fetch(URL, {
          method: "PUT",
          body: JSON.stringify(data),
          headers: {
            "Content-Type": "application/json"
          },
        }).then(
          response => {
            this.uploaded = true;
            console.log('image upload response > ', response)
          }
        )
      },
      uploadImage(event) {
          const reader = new FileReader();
          reader.readAsDataURL(event.target.files[0]);

          reader.onload = e =>{
              this.previewImage = e.target.result;
          };
        }    
      }
  }
</script>
