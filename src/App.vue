<template>
  <div id="app">
    <app-navbar></app-navbar>
    <main :style="{'background-position-x': navbarWidth}">
      <router-view/>
    </main>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'

export default {
  components: {
    appNavbar: Navbar
  },
  computed: {
    navbarWidth () {
      return this.$store.getters.getNavbarWidth
    }
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  font-family: 'Roboto', sans-serif;
  background-color: #424242;
}

/* The following code has been adapted from: (Google, n.d.c) */
body { font-size: 1rem; font-weight: 400; letter-spacing: 0.5; color: #fff; }
h1 { font-size: 6rem; font-weight: 300; letter-spacing: -1.5; }
h2 { font-size: 3.75rem; font-weight: 300; letter-spacing: -0.5; }
h3 { font-size: 3rem; font-weight: 400; letter-spacing: 0; font-family: 'Permanent Marker', cursive; opacity: .7;}
h4 { font-size: 2.125rem; font-weight: 400; letter-spacing: 0.25; }
h5 { font-size: 1.5rem; font-weight: 400; letter-spacing: 0; }
h6 { font-size: 1.5rem; font-weight: 500; letter-spacing: 0.15; }
.subtitle-1 { font-size: 1rem; font-weight: 400; letter-spacing: 0.15; }
.subtitle-2 { font-size: 0.875rem; font-weight: 500; letter-spacing: 0.1; }
.body-2 { font-size: 0.875rem; font-weight: 400; letter-spacing: 0.25; }
.button { font-size: 0.875rem; font-weight: 500; letter-spacing: 1.25; text-transform: uppercase; }
.caption { font-size: 0.75rem; font-weight: 400; letter-spacing: 0.4; }
.overline { font-size: 0.625rem; font-weight: 400; letter-spacing: 1.5; text-transform: uppercase; }
/* End of adapted code */

#app {
  display: flex;
}
#app > main {
  flex: 1 1 auto;
  background-image: url('./assets/background.png');
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: 100vw auto;
  background-position-x: 70%;
  background-position-y: 25%;
  margin-left: 2px;
}
@media only screen and (max-width: 1200px){
  #app > main {
    background-size: auto 150vh;
    background-position-x: 40%;
    background-position-y: 30%;
  }
}
.card, .card-action {
  background: #676565;
  position: relative;
  margin: 1rem;
  border-radius: 2px;
  /* The following code has been sourced from: (Thornton, S. n.d.) */
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  /* End of sourced code */
  overflow: hidden;
}
.card-action:hover, .card-popup {
  /* The following code has been sourced from: (Thornton, S. n.d.) */
  box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
  /* End of sourced code */
}
button:hover{
  /* The following code has been sourced from: (Thornton, S. n.d.) */
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  /* End of sourced code */
  color: rgba(255, 255, 255, .7);
  opacity: .9;
}
a {
  border: none;
  color: none;
  text-shadow: none;
  text-decoration: none;
}
ul {
  list-style-type: none;
  padding: 0;
  margin-left: 0;
}
ul a {
  text-decoration: none;
  color: inherit;
}
.button-row {
  padding-top: 1em;
  padding-bottom: 1em;
}
.button {
  padding: 1em;
  border-radius: 2px;
  outline-style: none;
  border: none;
  background-color: #D52916;
  color: #fff;
  font-weight: bold;
  text-decoration: none;
  cursor: pointer;
}
.button-small {
  padding: 0.2em;
  margin: 0 0.2em 0 0.2em;
}
.button-medium {
  padding: 0.6em;
  margin: 0 0.2em 0 0.2em;
}
.disabled-button {
  background-color: #424242;
  cursor: default;
}
.border-button {
  padding: 0.9em;
  background-color: #676565;
  color: #D52916;
  border: 1px solid #D52916;
}
input, select {
  padding: 1em 0 1em 1em;
  border-radius: 2px;
  border: 1px solid rgba(66, 66, 66, .5);
  background-color: #676565;
  outline: none;
  caret-color: #fff;
  color: #fff;
}
input:hover, select:hover {
  border: 1px solid #D52916;
  outline: none;
}
input:focus, select:focus {
  border: 1px solid #D52916;
  outline: none;
}
.floating-input {
  padding: .7em 0 .7em 0;
}
</style>
