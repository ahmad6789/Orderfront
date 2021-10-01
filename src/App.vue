<template>
  <div id="nav">
    <div id="mySidenav" class="sidenav">
      <a href="javascript:void(0)" class="closebtn" @click="closeNav"
        ><i class="bi bi-x-square"></i
      ></a>
      <router-link to="/">Product List</router-link>
      <router-link to="/OrderList">OrderList</router-link>
      <router-link to="/about">About</router-link>

      <a href="#">Clients</a>
      <a href="#">Contact</a>
    </div>

    <div id="main">
      <span style="font-size: 30px; cursor: pointer" @click="openNav"
        ><i class="bi bi-list-nested"></i
      ></span>
    </div>
  </div>
  <router-view />
</template>
<script>
export default {
  methods: {
    openNav() {
      document.getElementById("main").style.display = "none";
      document.getElementById("mySidenav").style.width = "250px";
    },

    closeNav() {
      document.getElementById("main").style.display = "block";
      document.getElementById("mySidenav").style.width = "0";
    },
  },
};
</script>
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

body {
  font-family: "Lato", sans-serif;
  background-color: #f4f7f9 !important;
}

.sidenav {
  height: 100%;
  width: 0;
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  background-color: #fff;
  overflow-x: hidden;
  transition: 0.8s;
  padding-top: 60px;
}

.sidenav a {
  padding: 8px 8px 8px 32px;
  text-decoration: none;
  font-size: 25px;
  color: #818181;
  display: block;
  transition: 0.3s;
}

.sidenav a:hover {
  color: #f1f1f1;
}

.sidenav .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 23px;
  margin-left: 50px;
}

#main {
  transition: margin-left 0.5s;
  padding: 16px;
  left: 16px;
  position: absolute;
}

@media screen and (max-height: 450px) {
  .sidenav {
    padding-top: 15px;
  }
  .sidenav a {
    font-size: 18px;
  }
}
</style>
