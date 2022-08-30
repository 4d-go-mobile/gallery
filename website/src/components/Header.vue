<template>
  <div class="fixedHeaderContainer">
    <div class="headerWrapper wrapper">
      <header>
        <a href="https://developer.4d.com/go-mobile/">
          <h2 class="headerTitle">4D go mobile</h2>
        </a>
        <div class="navigationWrapper navigationSlider">
          <nav class="slidingNav">
            <ul class="nav-site nav-site-internal">
              <li>
                <router-link class="link" to="/type/form-list"><b>List Forms</b></router-link>
              </li>
              <li>
                <router-link class="link" to="/type/form-detail"><b>Detail Forms</b></router-link>
              </li>
              <li>
                <router-link class="link" to="/type/form-login"><b>Login Forms</b></router-link>
              </li>
              <li>
                <router-link class="link" to="/type/formatter"><b>Formatters</b></router-link>
              </li>
              <li>
                <router-link class="link" to="/type/input-control"><b>Input Control</b></router-link>
              </li>
              <li>
                  <input @click="toggleTheme" onfocus='this.blur()' readonly='true' size='1' type="text" class="toggle_input" id="modeToggle" value="" />
              </li>
              <li class="navSearchWrapper reactNavSearchWrapper">
                <span
                  class="algolia-autocomplete"
                  style="position: relative; display: inline-block; direction: ltr;"
                >
                  <input
                    type="text"
                    id="search_input_react"
                    placeholder="Search"
                    title="Search"
                    class="aa-input"
                    autocomplete="off"
                    spellcheck="false"
                    role="combobox"
                    aria-autocomplete="list"
                    aria-expanded="false"
                    aria-labelledby="search_input_react"
                    aria-owns="algolia-autocomplete-listbox-0"
                    dir="auto"
                    style="position: relative; vertical-align: top;"
                    v-model="searchTerm"
                  />
                  <pre
                    aria-hidden="true"
                    style="position: absolute; visibility: hidden; white-space: pre; font-family: system-ui; font-size: 14px; font-style: normal; font-variant: normal; font-weight: 300; word-spacing: 0px; letter-spacing: normal; text-indent: 0px; text-rendering: auto; text-transform: none;"
                  ></pre>
                  <span
                    class="aa-dropdown-menu"
                    role="listbox"
                    id="algolia-autocomplete-listbox-0"
                    style="position: absolute; top: 100%; z-index: 100; display: none; left: 0px; right: auto;"
                  >
                    <div class="aa-dataset-1"></div>
                  </span>
                </span>
              </li>
            </ul>
          </nav>
        </div>
      </header>
    </div>
  </div>
</template>

<script type="text/javascript">
import { serverBus } from '../main.js';

export default {
  mounted() {
      const initUserTheme = this.getMediaPreference();
      this.setTheme(initUserTheme);
  },
  data() {
    return {
      searchTerm: "",
      userTheme: "light",
    };
  },
  watch: {
    searchTerm: function (newSearch, oldSearch) { 
    serverBus.$emit('search', this.searchTerm);
    },
    $route: "resetSearch"
  },
  methods: {
    resetSearch: function() {
      this.searchTerm = "";
    },
    setTheme(theme) {
      localStorage.setItem("user-theme", theme);
      this.userTheme = theme;
      if (theme === "dark_dimmed" || theme === "dark_high_contrast") {
        document.getElementsByTagName("html")[0].setAttribute("data-dark-theme", theme);
        document.getElementsByTagName("html")[0].setAttribute("data-color-mode", "dark");
        document.querySelector('#modeToggle').value = (theme === "dark_dimmed") ? "🔅": "👁‍🗨";
      } else {
        document.getElementsByTagName("html")[0].setAttribute("data-color-mode", theme);
        document.getElementsByTagName("html")[0].setAttribute("data-dark-theme", "dark");
        document.querySelector('#modeToggle').value = (theme === "dark") ? "🌜": "🌞";
      }
      // console.log("theme: "+theme)
    },
    getMediaPreference() {
      const hasDarkPreference = window.matchMedia(
        "(prefers-color-scheme: dark)"
      ).matches;
      if (hasDarkPreference) {
        return "dark";
      } else {
        return "light";
      }
    },
    toggleTheme(event) {
      const activeTheme = localStorage.getItem("user-theme");
      if (event.shiftKey) {
        this.setTheme("dark_dimmed");
      } else if (event.altKey) {
        this.setTheme("dark_high_contrast");
      } else if (activeTheme === "light") {
        this.setTheme("dark");
      } else {
        this.setTheme("light");
      }
    }
 }
} 
</script>

<style>
@media screen and (min-width: 1024px)
.navPusher {
    display: -webkit-box;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    flex-direction: column;
    min-height: calc(100vh - 50px);
    padding-top: 50px;
}
.navPusher {
  left: 0;
  min-height: 100%;
  padding-top: 100px;
  position: relative;
  z-index: 99;
}
@media only screen and (min-width: 1024px)
.navigationSlider .slidingNav ul li a {
    border: 0;
    color: white;
    display: -webkit-box;
    display: flex;
    font-size: 16px;
    font-size: 1em;
    font-weight: 300;
    height: 32px;
    line-height: 1.2em;
    margin: 0;
    padding: 6px 10px;
}
@media only screen and (min-width: 1024px)
.fixedHeaderContainer {
    flex-shrink: 0;
}
.fixedHeaderContainer header {
  display: -webkit-box;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  flex-flow: row nowrap;
  position: relative;
  text-align: left;
}

.toggle_input {
  padding: 6px;
  background: none;
  border: none;
  width: 30px;
}

</style>
