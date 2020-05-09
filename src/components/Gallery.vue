<template>
  <div class="container">
    <b-row class="mt-5" v-if="!images.length">
      <b-col>
        <b-spinner label="Loading images" variant="primary"></b-spinner>
      </b-col>
    </b-row>
    <div class="gallery">
      <ul id="content">
        <li v-for="image in images" :key="image.id">
        <img
          :src="image.url"
          :key="image.id"
          :alt="`Image of ${image.name} meme`"
          class="meme-thumb"
          v-b-modal.modal-1
          @click="imgToEdit = image.url"
        />
        </li>
      </ul>
    </div>
    <b-modal id="modal-1" size="xl" title="Editor">
      <Editor :imgToEdit="imgToEdit" />
    </b-modal>
  </div>
</template>
<script>

import Editor from "@/components/Editor.vue";
export default {
  name: "Gallery",
  components: { Editor },
  data() {
    return {
      images: [],
      imgToEdit: ""
    };
  },
  created() {
    fetch("https://api.imgflip.com/get_memes")
      .then(response => response.json())
      .then(response => (this.images = response.data.memes))
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.gallery {
 width: 1200px;
 margin: 0 auto;
}
 
#content {
 -moz-column-count: 6;
 -moz-column-gap: 0px;
 -webkit-column-count: 6;
 -webkit-column-gap: 0px;
 column-count: 6;
 column-gap: 0px;
 width: 1200px;
 list-style-type: none;
}
 
#content img{
 display: inline-block;
 margin-bottom: 0px;
 width: 100%;
 padding: 2px;
}

#modal-1 {

}


</style>


