<template>
  <div id="app" :class="{ 'dark-mode': isDarkMode }">
    <div class="content">
      <img ref="profilePicture" :src="profilePicture" alt="Patrice & Gunnar" class="profile-picture"
        @click="rotateProfilePicture" />
      <h2>Hey there! 👋</h2>
      <p class="introduction">
        I'm Patrice, a member of the cds-compiler team at SAP, where I help to build the
        backbone of the <a href="https://cap.cloud.sap/docs/" target="_blank" rel="noopener noreferrer">SAP Cloud
          Application Programming (CAP) Model</a>.
        Besides my duty in the cds-compiler team, I'm particularly excited about our work on the open source <a
          href="https://github.com/cap-js/cds-dbs" target="_blank" rel="noopener noreferrer">
          database services for CAP</a>.
      </p>
      <h2>In My Free Time</h2>
      <p class="introduction">
        When I'm not coding, you'll find me outdoors with my Husky, Gunnar, enjoying activities together such as
        canicross,
        bikejöring, or taking out the inline skates for a ride. I'm also a foodie who loves to cook and savor good
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
      |
      <a>
        <dark-mode-toggle class="toggle" :is-dark-mode="isDarkMode"
          @update:isDarkMode="isDarkMode = $event"></dark-mode-toggle>
      </a>
    </footer>
  </div>
</template>

<script>
import DarkModeToggle from './components/DarkModeToggle.vue';

export default {

  name: 'App',
  components: {
    DarkModeToggle
  },
  data() {
    return {
      profilePicture: require('@/assets/patrice.jpeg'),
      githubIcon: require('@/assets/github-mark.svg'),
      linkedInIcon: require('@/assets/linkedin-mark.svg'),
      mailIcon: require('@/assets/mail-mark.svg'),
      isDarkMode: false, // Theme tracking
      rotationCounter: 0,
    };
  },
  mounted() {
    this.setUpColorSchemeListener();
  },
  beforeUnmount() {
    // Remove the listener when the component is destroyed
    if (this.colorSchemeQuery) {
      this.colorSchemeQuery.removeListener(this.handleColorSchemeChange);
    }
  },
  methods: {
    setUpColorSchemeListener() {
      this.colorSchemeQuery = window.matchMedia('(prefers-color-scheme: dark)');
      this.handleColorSchemeChange(this.colorSchemeQuery); // Initial check
      this.colorSchemeQuery.addListener(this.handleColorSchemeChange); // Set up the listener
    },
    handleColorSchemeChange(e) {
      // Update isDarkMode based on the media query
      this.isDarkMode = e.matches;
    },
    toggleDarkMode(newValue) {
      this.isDarkMode = newValue;
    },
    rotateProfilePicture() {
      this.rotationCounter++;
      const picture = this.$refs.profilePicture;
      picture.style.animation = 'none';
      picture.style.animation = `${this.rotationCounter % 2 === 0 ? 'rotateImageVertically' : 'rotateImageHorizontally'} 1s ease-in-out`;
    }
  }
};
</script>


<style>
/* Reset CSS for html and body */
html,
body {
  padding: 0;
  margin: 0;
  height: 100%;
  /* Ensures that the body takes full viewport height */
  width: 100%;
  /* Ensures that the body takes full viewport width */
}

/* Ensure #app takes full available height */
#app {
  min-height: 100vh;
  /* vh units ensure the element takes at least the full height of the viewport */
  display: flex;
  flex-direction: column;
  text-align: center;
}

.content {
  flex: 1;
  /* Allows the content to expand and fill the space available, excluding header/footer */
}

body {
  font-family: 'Roboto', sans-serif;
}

.content {
  margin: 0 auto;
  max-width: 800px;
  padding: 0 10px;
}

.introduction {
  margin-top: 20px;
  font-size: 1.3em;
  line-height: 1.6;
}

.profile-picture {
  max-width: 200px;
  border-radius: 50%;
  animation: rotateImageVertically 1.5s ease-in-out;
  margin-top: 1em;
}

footer {
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;

  padding: 10px 0;
}

/* Responsive Media Query */
@media screen and (max-height: 400px) {
  footer {
    padding: 5px 0;
    flex-wrap: wrap;
    /* Wrap items if there isn't enough vertical space */
  }

  .social-icon {
    margin: 0.5em;
    /* Reduce margin around icons to save space */
  }
}

/* Additional media query for handling zooming issues */
@media (max-resolution: 2dppx) {
  .social-icon {
    width: 20px;
    /* Smaller icons at higher zoom levels */
  }
}

.social-icon {
  width: 24px;
  margin: 1em;
}

.toggle {
  margin: 1em;
}

/* Dark mode styles */
.dark-mode {
  background-color: #333;
  color: #fff;
}

.dark-mode a {
  color: #aad1f9;
}

.dark-mode footer {
  background-color: #222;
}

/* Dark Mode */
.dark-mode .social-icon,
toggle {
  filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(0deg) brightness(200%) contrast(100%);
}

/* Keyframes for rotation animations */
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
</style>
