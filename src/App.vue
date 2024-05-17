<template>
  <div id="app" :class="{ 'dark-mode': isDarkMode }">
    <div id="content">
      <img ref="profilePicture" :src="profilePicture" alt="Patrice & Gunnar" class="profile-picture"
        @click="rotateProfilePicture" />
      <h2>Hey there! <span id="shake-hand">👋</span></h2>
      <p class="introduction">
        I'm Patrice, a passionate software engineer 💻<br />
        <br />As a member of the <a href="https://www.npmjs.com/package/@sap/cds-compiler" target="_blank"
          rel="noopener noreferrer">cds-compiler</a> team at SAP, I help to build the
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
        <dark-mode-toggle id="toggle" :is-dark-mode="isDarkMode"
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
html,
body {
  padding: 0;
  margin: 0;
  height: 100%;
  width: 100%;
  font-family: 'Roboto', sans-serif;
  text-align: center;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}


#content {
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
  font-size: 1.5em;
  font-weight: 400;
  line-height: 1.5;
  margin: 0;
}

.profile-picture {
  max-width: 250px;
  border-radius: 50%;
  animation: rotateImageVertically 1.5s ease-in-out;
  margin-top: 1em;
  display: block;
  margin-left: auto;
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
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  z-index: 1000;
  height: 4em;
}

#shake-hand {
  display: inline-block;
  animation: wave 1.5s ease-in-out 1;
  animation-delay: 1s;
}

.social-icon {
  width: 24px;
  margin: 1em;
}

#toggle {
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

.dark-mode .social-icon,
toggle {
  filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(0deg) brightness(200%) contrast(100%);
}

/* Keyframes for rotation animations */
@keyframes wave {
  0% {
    transform: rotate(0deg);
  }

  15% {
    transform: rotate(14deg);
  }

  30% {
    transform: rotate(-8deg);
  }

  45% {
    transform: rotate(14deg);
  }

  60% {
    transform: rotate(-4deg);
  }

  75% {
    transform: rotate(10deg);
  }

  100% {
    transform: rotate(0deg);
  }
}

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
