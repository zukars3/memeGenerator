<template>
  <div class="row align-self-stretch">
    <div class="image" id="image-download">
      <vue-draggable-resizable
        v-for="(text, index) in texts"
        :key="index"
        id="top"
        :w="100"
        :h="30"
        :x="50"
        :y="50"
        :parent="true"
      >
        <p
          id="top-text"
          :style="{ fontSize: text.size + 'px', color: text.color }"
        >
          {{ text.text }}
        </p>
      </vue-draggable-resizable>
      <img :src="imgToEdit" :alt="`Image to edit`" />
    </div>
    <div class="col-6 input">
      <b-form>
        <b-form-group
          v-for="(text, index) in texts"
          :key="index"
          id="input-group-1"
          :label="'Text ' + (index + 1)"
          label-class="font-weight-bold"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            placeholder="Type text here"
            v-model="text.text"
          ></b-form-input
          >Font size:
          <vue-slider :min="15" :max="100" v-model="text.size"></vue-slider>
          <button type="button" class="btn btn-secondary" @click="text.size++">
            +
          </button>
          <button type="button" class="btn btn-secondary" @click="text.size--">
            -
          </button>
          {{ text.size }}
          <div>
            <v-swatches v-model="text.color" swatches="text-basic"></v-swatches>
          </div>
        </b-form-group>
      </b-form>
      <button
        style="margin-left: 60%"
        v-if="texts.length <= 3"
        type="button"
        class="btn btn-success"
        @click="addText"
      >
        Add text
      </button>
      <button
        style="margin-left: 15px"
        type="button"
        class="btn btn-primary"
        @click="download"
      >
        Download
      </button>
    </div>
  </div>
</template>
<script>
import VueSlider from "vue-slider-component";
import "vue-slider-component/theme/default.css";
import VueDraggableResizable from "vue-draggable-resizable";
import domtoimage from "dom-to-image";
import VSwatches from "vue-swatches";

export default {
  name: "Editor",
  components: {
    VueSlider,
    VueDraggableResizable,
    VSwatches,
  },
  props: {
    imgToEdit: {
      type: String,
    },
  },
  data() {
    return {
      texts: [],
    };
  },
  methods: {
    download: function () {
      domtoimage
        .toJpeg(document.getElementById("image-download"), { quality: 0.95 })
        .then(function (dataUrl) {
          var link = document.createElement("a");
          link.download = "my-image-name.jpeg";
          link.href = dataUrl;
          link.click();
        });
    },
    addText: function () {
      if (this.texts.length <= 3) {
        this.texts.push({
          text: "",
          size: 15,
          color: "#000000",
        });
      }
    },
  },
};
</script>

<style scoped>
.image {
  max-width: 50%;
  position: relative;
}

.image img {
  width: 100%;
  border: solid black 1px;
  display: block;
}

#top {
  position: absolute;
}

p {
  min-width: 200px;
  max-width: 500px;
  font-weight: bold;
}

.btn.btn-secondary {
  margin: 2px;
  width: 40px;
}
</style>