<template>
  <label class="switch">
    <input
      type="checkbox"
      :checked="isDarkMode"
      @change="onToggle"
    >
    <span class="slider round">
      <span
        v-show="!isDarkMode"
        class="emoji light"
      >🌝</span> <!-- Light mode emoji -->
      <span
        v-show="isDarkMode"
        class="emoji dark"
      >🌚</span> <!-- Dark mode emoji -->
    </span>
  </label>
</template>


<script>
export default {
    name: 'DarkModeToggle',
    props: {
        isDarkMode: Boolean
    },
    emits: ['update:isDarkMode'],
    methods: {
        onToggle(event) {
            this.$emit('update:isDarkMode', event.target.checked);
            // save theme preference in local storage
            localStorage.setItem('isDarkMode', event.target.checked);
            console.log('Dark mode is now:', event.target.checked ? 'enabled' : 'disabled');
        }
    }
};
</script>
<style scoped>
.switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
}

.switch input {
    opacity: 0;
    width: 0;
    height: 0;
}

.slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc; /* Light gray as inactive color */
    transition: 1s;
    border-radius: 34px;
}

.emoji {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    font-size: 1.5em;
    user-select: none; /* Prevent text selection */
}

.emoji.light {
    left: 4px;
}

.emoji.dark {
    right: 4px;
}

input:checked + .slider {
    background-color: #333; /* Darker for active state */
}

    input:focus + .slider {
        box-shadow: 0 0 1px #000; /* Optional focus style */
    }

.slider.round {
    border-radius: 34px;
}
</style>
