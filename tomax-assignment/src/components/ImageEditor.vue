<template>
  <div>
    <div ref="imageEditorContainer" class="image-editor">
      <div class="image-editor__images">
        <Resizer
          v-for="(image, index) in images"
          :key="index"
          class="image-edit"
          :width="image.width"
          :height="image.height"
          :index="index"
          :style="`top: ${image.y}px;
                   left: ${image.x}px;
                   transform: rotate(${image.rotate}deg);`"
          @resize="resizeImage"
        >
          <img
            :src="image.src"
            class="image"
          />
        </Resizer>
      </div>
      <canvas ref="imageEditor" class="image-editor__canvas"></canvas>
    </div>
    <button @click="downloadImage" class="tmx-button">Download Image</button>
  </div>
</template>

<script>
import Resizer from './Resizer.vue'

export default {
  name: 'ImageEditor',
  components: {
    Resizer,
  },
  props: {
    drop: Object
  },
  data: () => {
    return {
      ctx: null,
      images: [],
    }
  },
  methods: {
    dropImage() {
      let newImage = {
        src: this.$props.drop.photo,
        x: this.$props.drop.offsetX,
        y: this.$props.drop.offsetY,
        width: 80,
        height: 80,
        rotate: 0
      }

      this.images.push(newImage);
    },
    resizeImage(value) {
      this.images[value.index].width = value.width;
      this.images[value.index].height = value.height;
    },
    placeImageOnCanvas(image) {
      let imageItem = new Image();
      imageItem.src = image.src;
      // imageItem.crossOrigin = "Anonymous";
      this.ctx.drawImage(imageItem, image.x, image.y, image.width, image.height);
    },
    downloadImage() {
      for (let image of this.images) {
        this.placeImageOnCanvas(image);
      }
      this.images = [];
    },
  },
  mounted() {
    var canvas = this.$refs.imageEditor,
        container = this.$refs.imageEditorContainer,
        ctx;
    
    this.ctx = ctx = canvas.getContext("2d");
    
    canvas.width = container.offsetWidth;
    canvas.height = container.offsetHeight;

    var background = new Image();
    background.src = "https://images.pexels.com/photos/1591373/pexels-photo-1591373.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=400&w=600";
    
    // Make sure the image is loaded first otherwise nothing will draw.
    background.onload = function() {
      ctx.drawImage(background, 0, 0, container.offsetWidth, container.offsetHeight);
    }
  },

  watch: {
    drop: {
      deep: true,
      handler() {
        this.dropImage();
      }
    }
  }
}
</script>

<style scoped>
.image-editor {
  position: relative;
  height: 400px;
  border-radius: 10px;
  box-shadow: 0 1px 2px rgba(0,0,0, 0.2);
  overflow: hidden;
  margin-bottom: 20px;
  background-color: rgb(223, 223, 223);
}

.image-editor__images {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1;
  border: 3px solid transparent;
  border-radius: 10px;
  box-shadow: inset 0 0 0 0 rgba(0,0,0,0);
  transition: box-shadow .2s ease, border-color .2s ease;
}

.image-edit {
  position: absolute;
}

.image {
  width: 100%;
  height: 100%;
}

.image-editor__canvas {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  transition: opacity .2s ease;
}

.tmx-button {
  height: 40px;
  padding: 10px 20px;
  border: none;
  border-radius: 20px;
  background-color: rgb(33, 166, 126);
  color: #FFF;
  cursor: pointer;
}
</style>
