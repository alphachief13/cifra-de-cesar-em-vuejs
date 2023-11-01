<template>
  <TheHeader></TheHeader>
  <TheSection></TheSection>
</template>

<script>
import TheHeader from './components/TheHeader.vue'
import TheSection from './components/TheSection.vue';

export default {
  name: 'App',
  components: {
    TheHeader,
    TheSection
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');

*{
  margin: 0;
  padding: 0;
  font-family: 'Inter', sans-serif;
}
body{
  height: 100%;
  width: 100%;
}

</style>
