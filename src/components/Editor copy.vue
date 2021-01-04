<template>
  <div class="row align-self-stretch">
    <div class="image" id="image-download">
      <vue-draggable-resizable
        id="top"
        :w="100"
        :h="30"
        :x="50"
        :y="50"
        :parent="true"
      >
        <p id="top-text" :style="{ fontSize: topSize + 'px', color: topColor}">{{top}}</p>
      </vue-draggable-resizable>

      <vue-draggable-resizable
        id="bottom"
        :w="100"
        :h="30"
        :x="50"
        :y="300"
        :parent="true"
      >
      <p id="bottom-text" :style="{ fontSize: bottomSize + 'px', color: bottomColor}">{{bottom}}</p>
      </vue-draggable-resizable>

      <vue-draggable-resizable
        id="extra1"
        :w="100"
        :h="30"
        :x="50"
        :y="300"
        :parent="true"
        v-show="extra1show"
      >
        <p id="extra1-text" :style="{ fontSize: extra1Size + 'px', color: extra1Color}">{{extra1}}</p>
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
          <b-form-group
          id="input-group-3"
          label="Text 3"
          label-class="font-weight-bold"
          label-for="input-3"
          v-show="extra1show"
        >
          <b-form-input id="input-3" placeholder="Type text here" v-model="extra1"></b-form-input>Font size:
          <vue-slider :min="15" :max="100" v-model="extra1Size"></vue-slider>
          <button type="button" class="btn btn-secondary" @click="incrementFontSize('extra1')">+</button>
          <button type="button" class="btn btn-secondary" @click="decrementFontSize('extra1')">-</button>
          {{ extra1Size }}
          <div>
            <v-swatches v-model="extra1Color" swatches='text-basic'></v-swatches>
          </div>
        </b-form-group>
      </b-form>
      <button type="button" class="btn btn-success" @click="extra1show = true">Add one more text</button>
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
      extra1Size: 15,
      topColor: "#000000",
      bottomColor: "#000000",
      extra1Color: "#000000",
      extra1show: false
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
    download: function() {
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
  max-width: 100px;
  font-weight: bold;
}

.btn.btn-secondary {
  margin: 2px;
  width: 40px;
}
</style>