<template>
  <div class="row align-self-stretch">
    <div class="image" id="image-download">
      <vue-draggable-resizable
        id="top"
        :w="100"
        :h="30"
        :x="50"
        :y="50"
        @dragging="onDrag"
        :parent="true"
      >
        <p id="top-text" :style="{ fontSize: topSize + 'px', color: topColor}">{{top}}</p>
      </vue-draggable-resizable>

      <vue-draggable-resizable
        id="top"
        :w="100"
        :h="30"
        :x="50"
        :y="300"
        @dragging="onDrag"
        :parent="true"
      >
        <p id="bottom-text" :style="{ fontSize: bottomSize + 'px', color: bottomColor}">{{bottom}}</p>
      </vue-draggable-resizable>
      <img :src="imgToEdit" :alt="`Image to edit`" />
    </div>
    <div class="col-6 input">
      <b-form>
        <b-form-group
          id="input-group-1"
          label="Text 1"
          label-class="font-weight-bold"
          label-for="input-1"
        >
          <b-form-input id="input-1" placeholder="Type text here" v-model="top"></b-form-input>Font size:
          <vue-slider :min="15" :max="100" v-model="topSize"></vue-slider>
          <button type="button" class="btn btn-secondary" @click="incrementFontSize('top')">+</button>
          <button type="button" class="btn btn-secondary" @click="decrementFontSize('top')">-</button>
          {{ topSize }}
          <div>
            <v-swatches v-model="topColor" swatches='text-basic'></v-swatches>
          </div>
        </b-form-group>
        <b-form-group
          id="input-group-2"
          label="Text 2"
          label-class="font-weight-bold"
          label-for="input-2"
        >
          <b-form-input id="input-2" placeholder="Type text here" v-model="bottom"></b-form-input>Font size:
          <vue-slider :min="15" :max="100" v-model="bottomSize"></vue-slider>
          <button type="button" class="btn btn-secondary" @click="incrementFontSize('bottom')">+</button>
          <button type="button" class="btn btn-secondary" @click="decrementFontSize('bottom')">-</button>
          {{ bottomSize }}
          <div>
            <v-swatches v-model="bottomColor" swatches='text-basic'></v-swatches>
          </div>
        </b-form-group>
      </b-form>
      <button type="button" class="btn btn-primary" @click="download">Download</button>
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
    VSwatches
  },
  props: {
    imgToEdit: {
      type: String
    }
  },
  data() {
    return {
      top: "",
      bottom: "",
      topSize: 15,
      bottomSize: 15,
      width: 0,
      height: 0,
      x: 0,
      y: 0,
      topColor: "#000000",
      bottomColor: "#000000"
    };
  },
  methods: {
    incrementFontSize: function(where) {
      if (where == "top") {
        this.topSize++;
      } else this.bottomSize++;
    },
    decrementFontSize: function(where) {
      if (where == "top") {
        this.topSize--;
      } else this.bottomSize--;
    },
    onResize: function(x, y, width, height) {
      this.x = x;
      this.y = y;
      this.width = width;
      this.height = height;
    },
    onDrag: function(x, y) {
      this.x = x;
      this.y = y;
    },
    download: function() {
      console.log("wtf");

      domtoimage
        .toJpeg(document.getElementById("image-download"), { quality: 0.95 })
        .then(function(dataUrl) {
          var link = document.createElement("a");
          link.download = "my-image-name.jpeg";
          link.href = dataUrl;
          link.click();
        });
    }
  }
  // const link = document.createElement('a')
  //     link.setAttribute('href', output)
  //     link.setAttribute('download', `${this.filename}-meme.png`)
  //     // document.body.appendChild(link)
  //     link.click()
  // document.body.removeChild(link)
};
</script>

<style scoped>
.image {
  border: solid black 1px;
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
  max-width: 100px;
  font-weight: bold;
}

.btn.btn-secondary {
  margin: 2px;
  width: 40px;
}
</style>