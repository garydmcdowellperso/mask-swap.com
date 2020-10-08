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
.cancel-button {
  display: inline-block;
  font-size: 1.2rem;
  color: #fff;
  background-color: red;
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
  <div class="hero" v-if="masks.length > 0">
    <VueSlickCarousel :arrows="true" :dots="true">
      <div 
          v-for="(item, index) in masks">
        <img :src="item.image" />
        <p>
          <a class="nav-link action-button" @click="acceptMask(item.id)">
            Accept →
          </a>
          <a class="nav-link cancel-button" @click="rejectMask(item.id)">
            Reject X
          </a>
        </p>
      </div>
    </VueSlickCarousel>
  </div>
</template>
<script>
  import "isomorphic-fetch" // keep IE happy
  import VueSlickCarousel from 'vue-slick-carousel'
  import 'vue-slick-carousel/dist/vue-slick-carousel.css'
  // optional style for arrows & dots
  import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'
  //import { Carousel, Slide } from 'vue-carousel';
  export default {
    components: {
      VueSlickCarousel
    },
    mounted() {
      this.getUnmoderatedMasks();
    },
    data() {
      return {
        masks: null
      };
    }, 
    methods: {
      getUnmoderatedMasks(event) {
        const URL = 'https://simpatico.cloud/api/v1/getUnmoderatedMasks'; 

        fetch(URL, {
          method: "GET"
        })          
        .then(stream => stream.json())
        .then(data => this.masks = data.masks)
      },
      acceptMask(id) {
        const URL = 'https://simpatico.cloud/api/v1/moderateMask'; 

        const data = {
          id,
          moderated: true
        };

        fetch(URL, {
          method: "PUT",
          body: JSON.stringify(data),
          headers: {
            "Content-Type": "application/json"
          },
        }).then(
          response => {
            this.getUnmoderatedMasks();
            console.log('moderateMask > ', response)
          }
        )
      },
      rejectMask(id) {
        const URL = 'https://simpatico.cloud/api/v1/deleteMask'; 

        const data = {
          id          
        };

        fetch(URL, {
          method: "DELETE",
          body: JSON.stringify(data),
          headers: {
            "Content-Type": "application/json"
          },
        }).then(
          response => {
            console.log('deleteMask > ', response)
          }
        )
      }
    }
  }
</script>