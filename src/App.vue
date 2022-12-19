<template>
  <section
    class="h-screen bg-no-repeat bg-cover bg-[url('@/assets/img/bg.webp')] flex justify-center items-center"
  >
    <Card />
  </section>
</template>
<script>
import Card from "./components/Card.vue";

export default {
  components: {
    Card,
  },
};
</script>
