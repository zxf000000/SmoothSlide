<template>
  <div class="about">
    <h1>This is an about page</h1>
    <router-link to="/about/next">toNext</router-link> <br>
    <router-link to="/about/next/next1">to next next</router-link>
    <router-view></router-view>
  </div>
</template>
