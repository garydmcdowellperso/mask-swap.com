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
      <ImageUploader
        :debug="1"
        :maxWidth="512"
        :quality="0.7"
        :autoRotate=true
        outputFormat="verbose"
        :preview=false
        :className="['fileinput', { 'fileinput--loaded' : hasImage }]"
        :capture="false"
        accept="video/*,image/*"
        doNotResize="['gif', 'svg']"
        @input="setImage">
        <label for="fileInput" slot="upload-label">
        <figure>
          <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
              <path class="path1" d="M9.5 19c0 3.59 2.91 6.5 6.5 6.5s6.5-2.91 6.5-6.5-2.91-6.5-6.5-6.5-6.5 2.91-6.5 6.5zM30 8h-7c-0.5-2-1-4-3-4h-8c-2 0-2.5 2-3 4h-7c-1.1 0-2 0.9-2 2v18c0 1.1 0.9 2 2 2h28c1.1 0 2-0.9 2-2v-18c0-1.1-0.9-2-2-2zM16 27.875c-4.902 0-8.875-3.973-8.875-8.875s3.973-8.875 8.875-8.875c4.902 0 8.875 3.973 8.875 8.875s-3.973 8.875-8.875 8.875zM30 14h-4v-2h4v2z"></path>
          </svg>
        </figure>
        <span class="upload-caption">{{ hasImage ? 'Replace' : 'Upload' }}</span>
      </label>
      </ImageUploader>
    </p>
    <p v-if="previewImage && !uploaded">
      <a class="nav-link action-button" @click="sendImage($event)">
        Submit my mask →
      </a>
    </p>
    <p v-if="uploaded">
      Uploaded - Your mask may take a little time to appear as we moderate all contributions
    </p>
    <p v-if="uploaded">
      <a class="nav-link action-button" href="/view/">
        Look at some other contributions →
      </a>
    </p>
  </div>
</template>
<script>
  import "isomorphic-fetch" // keep IE happy
  export default {
    data() {
      return {
        hasImage: false,
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
      setImage(file) {
        this.hasImage = true;
        this.previewImage = file.dataUrl;
      },
      uploadImage(event) {
          const reader = new FileReader();
          reader.readAsDataURL(event.target.files[0]);

          reader.onload = e =>{
            this.resizeBase64Img(e.target.result, 500, 500).then((newImg) => {
                this.previewImage = newImg;
            });
            //this.previewImage = e.target.result;
          };
        }    
      }
  }
</script>
