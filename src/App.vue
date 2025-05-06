<template>
  <div
    id="app"
    :class="{ 'dark-mode': isDarkMode }"
  >
    <div id="content">
      <img
        id="profile-picture"
        :src="profilePicture"
        alt="Patrice & Gunnar"
        @click="rotateProfilePicture"
      >
      <h1>
        Hey there! <span
          id="shake-hand"
          @mouseover="shakeHand"
          @mouseleave="shakeHand"
        >👋</span>
      </h1>
      <p class="introduction">
        I'm Patrice,<br> a senior software engineer based in Heidelberg, Germany 🏰<br>
        <br>As a member of the <a
          href="https://www.npmjs.com/package/@sap/cds-compiler"
          target="_blank"
          rel="noopener noreferrer"
        >cds-compiler</a> team at SAP, I help to build the
        backbone of the <a
          href="https://cap.cloud.sap/docs/"
          target="_blank"
          rel="noopener noreferrer"
        >SAP Cloud
          Application Programming (CAP 🧢) Model</a>.
        Besides my duty in the cds-compiler team, I'm particularly excited about our work on the open source <a
          href="https://github.com/cap-js/cds-dbs"
          target="_blank"
          rel="noopener noreferrer"
        >
          database services for CAP</a>.
      </p>
      <h2>In My Free Time</h2>
      <p class="introduction">
        When I'm not coding, you'll likely find me outdoors with my dog, Gunnar—canicrossing, hiking, bikejöring, or cruising on inline skates. I'm also a foodie who loves to cook and can't resist a well-made espresso.
      </p>
      <br>
      <p class="introduction">
        <strong>Welcome to my corner of the web! <span
          id="rocket"
          @mouseover="animateRocket"
          @mouseleave="animateRocket"
          @click="animateRocket"
        >🚀</span></strong>
      </p>
    </div>
    <footer>
      <a
        href="https://www.linkedin.com/in/patrice-bender-64a816118/"
        alt="LinkedIn"
        target="_blank"
        rel="noopener noreferrer"
      >
        <img
          :src="linkedInIcon"
          alt="LinkedIn"
          class="social-icon"
        >
      </a>
      |
      <a
        href="https://github.com/patricebender"
        target="_blank"
        rel="noopener noreferrer"
      >
        <img
          :src="githubIcon"
          alt="GitHub"
          class="social-icon"
        >
      </a>
      |
      <a href="mailto:info@patrice.codes">
        <img
          :src="mailIcon"
          alt="mail"
          class="social-icon"
        >
      </a>
      |
      <a>
        <dark-mode-toggle
          id="toggle"
          :is-dark-mode="isDarkMode"
          @click="shakeHand"
          @update:is-dark-mode="isDarkMode = $event"
        />
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
      waveCounter: 0,
      rocketIsAboutToStart: false,
    };
  },
  mounted() {
    this.setUpColorSchemeListener();
    document.title = 'Patrice Bender - Personal Website';
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
    animateRocket(e) {
      const rocket = document.getElementById('rocket');
      rocket.style.animation = 'none';
      rocket.offsetHeight; // Trigger reflow, browser does not re-render the animation otherwise

      if (e.type === 'mouseover') { // shake the rocket on hover
        rocket.style.animation = 'liftoff 0.2s ease-in infinite';
        return;
      }

      if (e.type === 'mouseleave' && !this.rocketIsAboutToStart) return;

      this.rocketIsAboutToStart = true
      rocket.style.animation = 'liftoff 0.2s ease-in 5, fly 1.5s ease-in-out 1s';
      // Reset the flag after the animation ends
      setTimeout(() => {
        this.rocketIsAboutToStart = false;
      }, 2000); // Total duration of the animations (liftoff + fly)
    },
    rotateProfilePicture() {
      this.rotationCounter++;
      const picture = document.getElementById('profile-picture');
      picture.style.animation = 'none';
      picture.style.animation = `${this.rotationCounter % 2 === 0 ? 'rotateImageVertically' : 'rotateImageHorizontally'} 1s ease-in-out`;
    },
    shakeHand(e) {
      const shakeHand = document.getElementById('shake-hand');
      shakeHand.style.animation = 'none';
      shakeHand.offsetHeight; // Trigger reflow, browser does not re-render the animation otherwise

      if (e.type === 'mouseleave') return;
      if (e.type === 'mouseover') {
        shakeHand.style.animation = 'wave 1.5s ease infinite';
        return;
      }

      shakeHand.style.animation = 'wave 1.5s ease-in-out';
      this.waveCounter++;
      if (this.waveCounter === 10) { // easter egg if dark mode is toggled often enough
        shakeHand.innerHTML = '🫰';
      }
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

/* unique elements */
#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

#content {
  flex: 1;
  margin: 0 auto;
  max-width: 800px;
  padding: 0 20px;
  padding-bottom: 5em;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

#profile-picture {
  max-width: 250px;
  border-radius: 50%;
  animation: rotateImageVertically 1.5s ease-in-out;
  margin-top: 1em;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

#toggle {
  margin: 1em;
}

#shake-hand {
  display: inline-block;
  animation: wave 1.5s ease-in-out 1 1s;
}

#rocket {
  display: inline-block;
  cursor: pointer;
  animation: liftoff 0.2s ease-in 8 5s;
}

/* classes */
.introduction {
  font-size: 1.4em;
  font-weight: 400;
  line-height: 1.5;
  margin: 0;
}

.social-icon {
  width: 24px;
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

/* Media queries */

/* slightly reduces size for mobile devices */
@media (max-width: 600px) {
  .profile-picture {
    max-width: 200px;
  }

  #introduction {
    font-size: 1em;
  }
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

@keyframes liftoff {
  0% {
    transform: translateX(0);
  }

  25% {
    transform: translateX(-5px);
  }

  50% {
    transform: translateX(0);
  }

  75% {
    transform: translateX(5px);
  }

  100% {
    transform: translateX(0);
  }
}

@keyframes fly {
  0% {
    transform: translateX(0);
  }

  100% {
    transform: translate(100vw, -100vh);
    ;
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
