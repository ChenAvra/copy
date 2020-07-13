<template>
  <div id="app">
    <ul class="nav nav-pills">
      <li>
        <router-link :to="{ name: 'main' }">Recipes Main Page</router-link>
      </li>
      <li><router-link :to="{ name: 'search' }">Search</router-link></li>
      <li v-if="!$root.store.username">
        <router-link :to="{ name: 'register' }">Register</router-link>
      </li>
      <li v-if="!$root.store.username">
        <router-link :to="{ name: 'login' }">Login</router-link>
      </li>
      <li v-if="!$root.store.username"><a>Hello Guest!</a></li>
      <li v-if="$root.store.username"><a>My Area</a></li>

      <li v-if="$root.store.username"><a @click="Logout">Logout</a></li>
    </ul>

    <div
      class="header"
      style="background-image: url('https://res.cloudinary.com/dw5rvx9m6/image/upload/v1594561547/BT1-HEADER-RECIPES-1_whyhxi.png'); background-size: 100% 100%;"
    ></div>
    <!-- <div id="nav">
      <router-link :to="{ name: 'main' }">Recipes Main Page</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <router-link :to="{ name: 'favorites' }">favorite</router-link>|
       <router-link :to="{ name: 'myRecipes' }">favorite</router-link>|
      {{ !$root.store.username }}
      <span v-if="!$root.store.username">
        Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
      </span>
       <router-link :to="{ name: 'familyRecipes' }">family recipes</router-link>|
    </div> -->
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
  },
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.header {
  padding: 180px;
  text-align: center;
  background: #1abc9c;
  color: white;
  font-size: 30px;
}
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #deedde;
}

li {
  float: left;
  text-align: center;
  font-weight: bold;
  text-decoration: none;
}
li a {
  display: block;
  color: rgb(177, 169, 169);
  text-align: center;
  font-weight: bold;
  padding: 14px 16px;
  text-decoration: none;
}
li a:hover {
  background-color: #ffcccc;
}
</style>
