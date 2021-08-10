<template>
  <div class="container">
    <section class="user-images">
      <ImageEditor class="user-images__editor" :drop="drop"></ImageEditor>
      <div class="user-images__user-list">
        <User
          v-for="user in users"
          :key="user.user"
          :name="user.name"
          :code="user.user"
          :photo="user.picture"
          @drop="setUserImageOnCanvas"
        ></User>
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
      users: [],
      drop: {},
    }
  },
  methods: {
    setUserImageOnCanvas(value) {
      this.drop = value;
    }
  },
  async mounted() {
    try {
      const response = await fetch('http://www.mocky.io/v2/59bd9a773c00001303529fe0', {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json'
        }
      });
      const jsonResponse = await response.json();
      this.users = jsonResponse.users;
    } catch (err) {
      console.log(">>>>>>>>>> Error loading users", err);
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
