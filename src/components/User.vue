<template>
  <div class="user">
    <div ref="photo" class="user__photo">
      <img
        :src="photo"
        :data-code="code"
        :alt="name"
        draggable="true"
        v-on:dragstart="onDragStart"
      />
    </div>
    <div class="user__details">
      <h3>{{ name }}</h3>
      <p>{{ code }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'User',
  props: {
    name: String,
    code: Number,
    photo: String
  },
  data: () => {
    return {
      mouseOffsetX: null,
      mouseOffsetY: null,
    }
  },
  methods: {
    onDragStart:function(e) {
      e.dataTransfer.setData('text/plain', null);

      const rect = e.target.getBoundingClientRect();

      this.mouseOffsetX = e.clientX - rect.x;
      this.mouseOffsetY = e.clientY - rect.y;
    }
  },
  mounted() {
    let dragged,
        code,
        context = this;

    document.addEventListener('dragstart', function (e) {
      dragged = e.target.src;
      code = e.target.dataset.code;
    }, false);
    
    document.addEventListener('dragend', function () {
      dragged = null;
      code = null;
    }, false);

    document.addEventListener("dragover", function (e) {
      e.preventDefault();
    }, false);

    document.addEventListener("dragenter", function (e) {
      // highlight potential drop target when the draggable element enters it
      if ( e.target.className == "image-editor__images" ) {
          e.target.style.boxShadow = "inset 0 0 0 20px rgba(0, 0, 0, 0.2)";
          e.target.style.borderColor = "rgb(33, 166, 126)";
      }
    }, false);
    
    document.addEventListener("dragleave", function (e) {
      // highlight potential drop target when the draggable element enters it
      if ( e.target.className == "image-editor__images" ) {
          e.target.style.boxShadow = "inset 0 0 0 0 rgba(0, 0, 0, 0)";
          e.target.style.borderColor = "transparent";
      }
    }, false);

    document.addEventListener("drop", function (e) {
      // prevent default action (open as link for some elements)
      e.preventDefault();
      
      if ( e.target.className == "image-editor__images" ) {
        e.target.style.boxShadow = "inset 0 0 0 0 rgba(0, 0, 0, 0)";
        e.target.style.borderColor = "transparent";
        
        if (parseInt(context.code) === parseInt(code)) {
          context.$emit('drop', {
            photo: dragged,
            offsetX: e.layerX - context.mouseOffsetX,
            offsetY: e.layerY - context.mouseOffsetY
          });
        }
      }
    }, false);
  }
}
</script>

<style scoped>
.user {
  height: 100px;
  padding: 10px;
  margin-bottom: 10px;
  display: flex;
  justify-content: flex-start;
  border-radius: 10px;
  box-shadow: 0 1px 2px rgba(0,0,0, 0.2);
}

.user__photo {
  flex: 0 0 80px;
  height: 80px;
  width: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  border-radius: 8px;
  margin-right: 10px;
  background-color: rgb(223, 223, 223);
}

.user__photo img {
  max-width: 80px;
  height: auto;
  cursor: grab;
  cursor: -moz-grab;
  cursor: -webkit-grab;
}

.user__photo:active img {
  cursor: grabbing;
  cursor: -moz-grabbing;
  cursor: -webkit-grabbing;
}

.user__details {
  flex: 1;
}

.user__details h3 {
  font-size: 24px;
  font-weight: 700;
  color: rgb(33, 166, 126);
}

.user__details p {
  font-size: 16px;
  font-weight: 100;
}
</style>
