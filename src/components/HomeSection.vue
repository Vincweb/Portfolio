<template>
  <section class="home">
    <div class="background-image reveal-background" :class="{'reveal-background-visible': showBackground }" />
    <div class="logo-container reveal">
      <img src="../assets/logo.png" alt="Logo" class="logo" />
    </div>
    <div class="arrow-container reveal">
      <i class="fas fa-chevron-down"></i>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const showBackground = ref(false);

onMounted(() => {
    setTimeout(() => {
      showBackground.value = true;
    }, 1000);

    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if(entry.intersectionRatio > 0) {
          entry.target.classList.add('reveal-visible');
          observer.unobserve(entry.target);
        }
      })
    })
    document.querySelectorAll('.reveal').forEach((r) => {
      observer.observe(r);
    })
})
</script>

<style scoped>
.home {
  background-color: black;
  height: 100vh;
  overflow: hidden;
}
.logo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 80%;
}
.logo {
  max-height: 400px;
}
.reveal{
  opacity: 0;
  transform: translateY(-30px)
}
.reveal-visible{
  opacity: 1;
  transform: translateY(0);
  transition: 2s cubic-bezier(.5, 0, 0, 1);
}
.reveal-background {
  opacity: 0;
}
.reveal-background-visible {
  opacity: 0.4;
  transition: 2s;
}
.background-image {
  position: absolute;
  top: 0px;
  right: 0px;
  bottom: 0px;
  left: 0px;
  height: 100vh;
  background-image: url('../assets/background.jpg');
  background-size: cover;
  background-position: center;
}
.arrow-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 20%;
}
.arrow-container i {
  font-size: 3rem;
  color: white;
}

.arrow-container:hover{
  animation: arrow-bounce 2s infinite ease-in-out;
}

@keyframes arrow-bounce {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
  100% {
    transform: translateY(0);
  }
}
</style>
