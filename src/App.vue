<template>
  <div id="app">
    <div class="content">
      <img ref="profilePicture" :src="profilePicture" alt="Patrice & Gunnar" class="profile-picture"
        @click="rotateProfilePicture" id="profile-picture" />
      <h2>Hey there! 👋</h2>
      <p class="introduction">
        I'm Patrice, a member of the cds-compiler team at SAP, where I help to build the
        backbone of the <a href="https://cap.cloud.sap/docs/">SAP Cloud Application Programming (CAP) Model</a>.
        Besides my duty in the cds-compiler team, I'm particularly excited about our work on the open source <a
          href="https://github.com/cap-js/cds-dbs" target="_blank" rel="noopener noreferrer">
          database services for CAP</a>.
      </p>
      <h2>In My Free Time</h2>
      <p class="introduction">
        When I'm not coding, you'll find me outdoors with my Husky, Gunnar, enjoying activities together such as
        canicross,
        bikejöring or taking out the inline skates for a ride. I'm also a foodie who loves to cook and savor good
        coffee.
      </p>

      <p class="introduction">
        <strong>Welcome to my corner of the web!</strong>
      </p>
    </div>
    <footer>
      <a href="https://www.linkedin.com/in/patrice-bender-64a816118/" target="_blank" rel="noopener noreferrer">
        <img :src="linkedInIcon" alt="LinkedIn" class="social-icon" />
      </a>
      |
      <a href="https://github.com/patricebender" target="_blank" rel="noopener noreferrer">
        <img :src="githubIcon" alt="GitHub" class="social-icon" />
      </a>
      |
      <a href="mailto:info@patrice.codes">
        <img :src="mailIcon" alt="mail" class="social-icon" />
      </a>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      profilePicture: require('@/assets/patrice.jpeg'),
      githubIcon: require('@/assets/github-mark.svg'),
      linkedInIcon: require('@/assets/linkedin-mark.svg'),
      mailIcon: require('@/assets/mail-mark.svg'),
      huskyIcon: require('@/assets/husky.svg'),
      rotationCounter: 0, // Add this line
    };
  },
  methods: {
    rotateProfilePicture() {
      this.rotationCounter++; // Increment the counter
      const picture = this.$refs.profilePicture; // Access the image using a ref
      // Force reflow to ensure the animation is recognized as new
      picture.style.animation = 'none';
      picture.offsetHeight; // Trigger reflow
      // Apply the animation with the rotationCounter to ensure uniqueness
      picture.style.animation = `${this.rotationCounter % 2 === 0 ? 'rotateImageVertically' : 'rotateImageHorizontally'} 1s ease-in-out`;
    }
  }
};
</script>

<style>
body {
  font-family: 'Roboto', sans-serif;
}

#app {
  text-align: center;
  margin-top: 50px;
}

.content {
  margin: 0 auto;
  /* Center the content */
  max-width: 800px;
  /* Limit the width to 800px */
  padding: 0 10px;
  /* Add some padding on the sides */
}

.introduction {
  margin-top: 20px;
  font-size: 1.3em;
  /* Increase the font size */
}

/* Optional: Adjust the line-height for better readability */
.introduction {
  line-height: 1.6;
}


/* Keyframes for rotation animation */
/* Keyframes for vertical rotation animation */
@keyframes rotateImageVertically {
  from {
    transform: rotateY(0deg);
  }

  to {
    transform: rotateY(360deg);
  }
}

@keyframes rotateImageHorizontally {
  from {
    transform: rotateX(0deg);
  }

  to {
    transform: rotateX(360deg);
  }
}

.profile-picture {
  max-width: 200px;
  border-radius: 50%;
  /* Apply the vertical rotation animation */
  animation: rotateImageVertically 1.5s ease-in-out;
}

.introduction {
  margin-top: 20px;
}

footer {
  margin-top: 40px;
  padding: 20px;
  background-color: #f0f0f0;
}

.social-icon {
  width: 24px;
  /* Adjust size as needed */
  vertical-align: middle;
  margin-left: 5px;
}
</style>
