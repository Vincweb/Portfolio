<template>
  <div>
    <HomeSection />
    <GameOfLifeSection />
    <ContactSection />
  </div>
</template>

<script>
import HomeSection from '@/components/HomeSection.vue';
import GameOfLifeSection from '@/components/GameOfLifeSection.vue'
import ContactSection from '@/components/ContactSection.vue'

export default {
  name: 'App',
  components: {
    HomeSection,
    GameOfLifeSection,
    ContactSection
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: 'Roboto';
  background-color: black;
}
</style>
