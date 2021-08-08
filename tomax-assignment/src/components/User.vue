<template>
  <div class="user">
    <div class="user__photo">
      <img :src="photo" :alt="name" />
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
  cursor: grab;
  cursor: -moz-grab;
  cursor: -webkit-grab;
}

.user__photo:active {
  cursor: grabbing;
  cursor: -moz-grabbing;
  cursor: -webkit-grabbing;
}

.user__photo img {
  max-width: 80px;
  height: auto;
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
