<template>
  <Suspense>
    <template #default>
      <HomeComp></HomeComp>
    </template>
    <template #fallback>
      <SplashScreenComp></SplashScreenComp>
    </template>
  </Suspense>
</template>
<!-- <SplashScreenComp></SplashScreenComp> -->
<script>
import { defineAsyncComponent } from "vue";
import SplashScreenComp from "./components/SplashScreenComp.vue";

export default {
  name: "App",
  components: {
    HomeComp: defineAsyncComponent(() => new Promise((resolve) => {
        setTimeout(() => {
            resolve(import("./components/HomeComp.vue"));
        }, 2500);
    })),
    SplashScreenComp
},
};
</script>

<style>
html,
body,
.app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}
* {
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
</style>
