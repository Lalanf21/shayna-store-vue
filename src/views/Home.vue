<template>
  <div class="home">
     <Header />

     <Slider />

     <WomanProduct />

     <InstaShayna />
     
     <PartnerShayna />
     
     <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import Slider from "@/components/Slider.vue";
import WomanProduct from "@/components/WomanProduct.vue";
import InstaShayna from "@/components/InstaShayna.vue";
import PartnerShayna from "@/components/PartnerShayna.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: 'Home',
  components: {
    Header,
    Slider,
    WomanProduct,
    PartnerShayna,
    InstaShayna,
    Footer
  }
}
</script>
