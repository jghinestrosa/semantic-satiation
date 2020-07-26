<template>
  <div class="text-to-speech">
    <div class="speech-input">
      <input type="text" placeholder="Type something" class="input-text" autofocus v-model="textToSpeech" v-on:keyup.13="onEnter" />
      <div class="select-wrapper">
        <select id="lang" class="select-language" name="lang" v-model="language" :disabled="speaking">
          <option v-for="lang in languages" :key="lang" :value="lang">{{lang}}</option>
        </select>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="select-icon"
        >
          <polyline points="6 9 12 15 18 9"></polyline>
        </svg>
      </div>
    </div>
    <section class="buttons">
      <button v-show="!speaking" :disabled="!readyToSpeak" v-on:click="speak" class="button button-speak">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          aria-hidden="true"
          focusable="false"
        >
          <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon>
          <path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"></path>
        </svg>
        <span class="sr-only">Speak</span>
      </button>
      <button v-show="speaking" :disabled="!speaking" v-on:click="stop" class="button button-stop">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          aria-hidden="true"
          focusable="false"
        >
          <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon>
          <line x1="23" y1="9" x2="17" y2="15"></line>
          <line x1="17" y1="9" x2="23" y2="15"></line>
        </svg>
        <span class="sr-only">Stop</span>
      </button>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      textToSpeech: '',
      language: '',
      voices: [],
      speaking: false
    };
  },
  mounted() {
    this.loadVoices();
    window.speechSynthesis.onvoiceschanged = this.loadVoices;
  },
  computed: {
    readyToSpeak() {
      return !!this.textToSpeech && this.languages.length && !this.speaking;
    },
    languages() {
      if (!this.voices.length) {
        return [];
      }

      const languages = this.voices.map(({ lang }) => lang);
      return languages
        .filter((language, index) => languages.indexOf(language) === index)
        .sort();
    },
    voice() {
      if (!this.voices || !this.voices.length) {
        return null;
      }

      return this.voices.find(({ lang }) => lang === this.language);
    }
  },
  watch: {
    languages(value) {
      if (!value.length) {
        return;
      }

      this.language = value[0];
    }
  },
  methods: {
    loadVoices() {
      this.voices = window.speechSynthesis.getVoices();
    },
    speak() {
      const utter = new SpeechSynthesisUtterance(this.textToSpeech);
      utter.addEventListener('end', () => {
        this.speaking && window.speechSynthesis.speak(utter);
      });
      utter.voice = this.voice;
      window.speechSynthesis.speak(utter);
      this.speaking = true;
    },
    stop() {
      window.speechSynthesis.pause();
      window.speechSynthesis.cancel();
      this.speaking = false;
    },
    onEnter() {
      if (this.readyToSpeak) {
        this.speak();
        return;
      }

      if (this.speaking) {
        this.stop();
      }
    }
  }
};
</script>

<style scoped>
.text-to-speech {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  margin-top: 1.5em;
}

.speech-input {
  border: 1px solid lightgray;
  border-right: none;
  display: inline-block;
  width: 75%;
  position: relative;
  border-top-left-radius: 4px;
  border-bottom-left-radius: 4px;
}

.input-text {
  font-size: 1em;
  border: 0;
  outline: none;
  width: 100%;
  box-sizing: border-box;
  padding: 0.5em;
  padding-right: 5.1em;
  border-top-left-radius: 4px;
  border-bottom-left-radius: 4px;
}

.select-wrapper {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0.2em;
  display: inline-block;
  cursor: pointer;
  color: black;
}

.select-language {
  border: none;
  /* outline: none; */
  background: white;
  font-size: 0.8em;
  -moz-appearance: none;
  -webkit-appearance: none;
  cursor: pointer;
  position: relative;
  width: 5.1em;
  z-index: 2;
  background: transparent;
  height: 100%;
}

.select-language:hover, .select-wrapper:hover {
  color: #2c3e50;
}

.select-language + svg {
  height: 100%;
}

.select-language:disabled + svg {
  color: gray;
}

.select-icon {
  position: absolute;
  right: 0;
  z-index: 1;
}

.button {
  height: 100%;
  color: white;
  border: 0;
  transition-property: background;
  transition-duration: 0.5s;
  border-top-right-radius: 4px;
  border-bottom-right-radius: 4px;
  padding: 0;
  width: 4em;
}

.button:not(:disabled) {
  cursor: pointer;
}

.button-speak {
  background: #36a22a;
}

.button-speak:disabled {
  background: gray;
}

.button-stop {
  background: #dc3535;
}

.sr-only:not(:focus):not(:active) {
  clip: rect(0 0 0 0);
  clip-path: inset(100%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}
</style>
