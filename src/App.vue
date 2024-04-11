<template>
  <div id="app">
    <div class="content">
      <img :src="profilePicture" alt="Your Name" class="profile-picture" />
      <h2>Hey there! 👋</h2>
      <p class="introduction">
        I'm Patrice, a member of the cds-compiler team at SAP, where I help to build the
        backbone of the <a href="https://cap.cloud.sap/docs/">SAP Cloud Application Programming (CAP) Model</a>.
        Besides my duty in the cds-compiler team, I'm particularly excited about our work on the open source <a href="https://github.com/cap-js/cds-dbs" target="_blank" rel="noopener noreferrer">
        database services for CAP</a>.
      </p>
      <h2>In My Free Time</h2>
      <p class="introduction">
        When I'm not coding, you'll find me outdoors with my Husky, Gunnar, enjoying activities together such as canicross,
        bikejöring or taking out the inline skates for a ride. I'm also a foodie who loves to cook and savor good coffee.
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
    };
  },
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
  margin-bottom: 20px;
}

.profile-picture {
  max-width: 200px;
  border-radius: 50%;
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
