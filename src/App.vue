<template>
  <HeaderContent></HeaderContent>
  <MainContent></MainContent>
  <FooterContent></FooterContent>
</template>

<script>
import HeaderContent from "@/components/HeaderContent.vue";
import FooterContent from "@/components/FooterContent.vue";
import MainContent from "@/components/MainContent.vue";

export default {
  components: {
    HeaderContent,
    FooterContent,
    MainContent,
  },
};
</script>

<style>
body {
  margin: 0%;
  background-color: rgb(49 149 2);
}
</style>
