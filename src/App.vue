<template>
  <div id="app" :class="{ 'dark-mode': isDarkMode }">
    <div class="content">
      <img ref="profilePicture" :src="profilePicture" alt="Patrice & Gunnar" class="profile-picture"
        @click="rotateProfilePicture" />
      <h2>Hey there! 👋</h2>
      <p class="introduction">
        I'm Patrice, a passionate software engineer 💻<br />
        <br />As a member of the <a href="https://www.npmjs.com/package/@sap/cds-compiler" arget="_blank" rel="noopener noreferrer">cds-compiler</a> team at SAP, I help to build the
        backbone of the <a href="https://cap.cloud.sap/docs/" target="_blank" rel="noopener noreferrer">SAP Cloud
          Application Programming (CAP 🧢) Model</a>.
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
        <strong>Welcome to my corner of the web! 🚀</strong>
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
      // check if user has isDarkMode preference saved to local storage
      const isDarkMode = localStorage.getItem('isDarkMode');
      if (isDarkMode) {
        this.isDarkMode = JSON.parse(isDarkMode);
      }
      else {
        this.handleColorSchemeChange(this.colorSchemeQuery); // Initial check
      }
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
  width: 100%;
  font-family: 'Roboto', sans-serif;
  text-align: center;
}

/* Ensure #app takes full available height */
#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}


.content {
  flex: 1;
  margin: 0 auto;
  max-width: 800px;
  padding: 0 10px;
  padding-bottom: 5em;
  display: flex;
  flex-direction: column;
  justify-content: center;
}


.introduction {
  font-size: 1.3em;
  line-height: 1.5;
  margin: 0;
}

.profile-picture {
  max-width: 250px;
  border-radius: 50%;
  animation: rotateImageVertically 1.5s ease-in-out;
  margin-top: 1em;
  display: block;      /* Makes the img a block-level element */
  margin-left: auto;   /* Auto margin on both sides will center the image */
  margin-right: auto;
}
/* slightly reduces size for mobile devices */
@media (max-width: 600px) {
  .profile-picture {
    max-width: 200px;
  }
}

footer {
  position: fixed;
  /* Fixes the footer's position relative to the viewport */
  bottom: 0;
  /* Aligns the footer at the bottom of the viewport */
  left: 0;
  /* Aligns the footer to start from the left side of the viewport */
  width: 100%;
  /* Ensures the footer extends across the full width of the viewport */
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  z-index: 1000;
  /* Ensures the footer stays above other content */
  height: 4em;
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
