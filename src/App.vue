<template>
  <div id="app">
    <main class="content-wrapper">
      <section class="content">
        <h1>Semantic Satiation</h1>
        <blockquote cite="https://en.wikipedia.org/wiki/Semantic_satiation">
          <h2>A psychological phenomenon in which repetition causes a word or phrase to temporarily lose meaning for the listener<sup><a target="_blank" href="https://en.wikipedia.org/wiki/Semantic_satiation">[1]</a></sup></h2>
        </blockquote>
        <SpeechInput />
      </section>
    </main>
    <footer>
      <a target="_blank" href="https://en.wikipedia.org/wiki/Semantic_satiation">About Semantic Satiation</a>
      <a target="_blank" href="">About the project</a>
      <a target="_blank" href="https://www.youtube.com/watch?v=0vv0ru44NQA">Bowl</a>
    </footer>
  </div>
</template>

<script>
import SpeechInput from './components/SpeechInput';

export default {
  name: 'App',
  components: {
    SpeechInput
   }
};
</script>

<style>
@font-face {
  font-family: 'Great Vibes';
  src: url('./assets/GreatVibes-Regular.ttf') format('truetype');
}

html, body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  width: 100vw;
  height: 100vh;
}

.content-wrapper {
  height: 95vh;
  width: 100vw;
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-items: center;
}

.content {
  min-height: 40vh;
  margin: 1em;
}

h1 {
  font-size: 3.5em;
  margin: 0;
  text-align: center;
  font-family: Great Vibes;
}

h2 {
  font-size: 0.7em;
  text-align: center;
}

footer {
  display: block;
  text-align: center;
}

footer a:not(:last-child)::after {
  content: '|';
  padding: 0.5em;
}
</style>
