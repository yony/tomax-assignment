<template>
  <div>
    <div ref="imageEditorContainer" class="image-editor">
      <canvas ref="imageEditor" class="image-editor__canvas"></canvas>
    </div>
    <button class="tmx-button">Download Image</button>
  </div>
</template>

<script>
export default {
  name: 'ImageEditor',
  mounted() {
    var canvas = this.$refs.imageEditor,
        container = this.$refs.imageEditorContainer,
        ctx = canvas.getContext("2d");

    canvas.width = container.offsetWidth;
    canvas.height = container.offsetHeight;
    
    var background = new Image();
    background.src = "https://images.pexels.com/photos/1591373/pexels-photo-1591373.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=400&w=600";
    
    // Make sure the image is loaded first otherwise nothing will draw.
    background.onload = function(){
      ctx.drawImage(background, 0, 0, container.offsetWidth, container.offsetHeight);   
    }
  },
}
</script>

<style scoped>
.image-editor {
  height: 400px;
  border-radius: 10px;
  box-shadow: 0 1px 2px rgba(0,0,0, 0.2);
  overflow: hidden;
  margin-bottom: 20px;
  background-color: rgb(223, 223, 223);
}

.image-editor__canvas {
  width: 100%;
  height: 100%;  
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
