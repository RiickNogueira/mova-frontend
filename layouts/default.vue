
<template>
  <v-app>
    <Header />
    <Content />
    <Footer />
  </v-app>
</template>

<script>
import Header from "@/components/layout/Header";
import Content from "@/components/layout/Content";
import Footer from "@/components/layout/Footer";
export default {
  name: "App",
  components: { Header, Content, Footer },
};
</script>
