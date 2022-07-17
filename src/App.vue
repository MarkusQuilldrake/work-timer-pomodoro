<template>
  <div id="app">
    <nav style="display: none">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view/>
  </div>
</template>

<style lang="scss">
  body{
    margin: 0;
    padding: 0;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #232328;
    color: #f2f2f8;
    #app{
      width: Auto;
    }
  }
</style>
