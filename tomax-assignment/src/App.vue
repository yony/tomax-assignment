<template>
  <div class="container">
    <section class="user-images">
      <ImageEditor class="user-images__editor"></ImageEditor>
      <div class="user-images__user-list">
        <User v-for="user in users" :key="user.user" :name="user.name" :code="user.user" :photo="user.picture" />
      </div>
    </section>
  </div>
</template>

<script>
import ImageEditor from './components/ImageEditor.vue'
import User from './components/User.vue'

export default {
  name: 'App',
  components: {
    ImageEditor,
    User,
  },
  data: () => {
    return {
      users: [
        {
          "user":1,
          "name" :"משה",
          "picture": "https://image.freepik.com/free-icon/user-silhouette_318-79814.jpg"
        },
        {
          "user":2,
          "name" :"חיים",
          "picture": "https://thumb1.shutterstock.com/display_pic_with_logo/1442480/564112600/stock-vector-businessman-icon-564112600.jpg"
        },
        {
          "user":3,
          "name" :"אבי",
          "picture": "https://thumb1.shutterstock.com/display_pic_with_logo/2239973/365127620/stock-vector-man-icon-365127620.jpg"
        }
      ]
    }
  }
}
</script>

<style>
/* CSS RESET */
/* Box sizing rules */
*,
*::before,
*::after {
  box-sizing: border-box;
}

/* Remove default padding */
ul[class],
ol[class] {
  padding: 0;
}

/* Remove default margin */
body,
h1,
h2,
h3,
h4,
p,
ul[class],
ol[class],
li,
figure,
figcaption,
blockquote,
dl,
dd {
  margin: 0;
}

@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;500;700&display=swap');

/* Set core body defaults */
body {
  min-height: 100vh;
  scroll-behavior: smooth;
  text-rendering: optimizeSpeed;
  line-height: 1.5;
  font-family: 'Roboto', sans-serif;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
}

.user-images {
  display: flex;
  justify-content: space-between;
}

.user-images__editor {
  flex: 0 0 600px;
}

.user-images__user-list {
  flex: 1;
  margin-left: 20px;
}
</style>
