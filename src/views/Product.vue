<template>
  <div class="product">
     <Header />

     <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row text-left">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/">
                            <i class="fa fa-home"></i> Home
                        </router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="gambar_default" />
                            </div>
                            <div class="product-thumbs" v-if="productDetail.gallery.length > 0">
                                <carousel :nav="false" dot="false" class="product-thumbs-track ps-slider">

                                    <div 
                                    v-for="productPhoto in productDetail.gallery"
                                    :key="productPhoto.id"
                                    class="pt" @click="changeImage(productPhoto.photo)" :class="productPhoto.photo == gambar_default ? 'active' : '' " >
                                        <img :src="productPhoto.photo" alt="" />
                                    </div>

                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6 text-left">
                            <div class="product-details">
                                <div class="pd-title">
                                    <span>{{ productDetail.type }}</span>
                                    <h3>{{ productDetail.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p>
                                        {{ productDetail.description }}
                                    </p>
                                    <h4>${{ productDetail.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/cart" class="primary-btn pd-cart">Add To Cart</router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProduct/>
     
     <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import RelatedProduct from "@/components/RelatedProduct.vue";
import Footer from "@/components/Footer.vue";
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name: 'Product',
    components: {
        Header,
        Footer,
        RelatedProduct,
        carousel
    },
    data(){
        return {
            gambar_default:"",
            productDetail: []
        }
    },
    methods:{
        changeImage(urlImg){
            this.gambar_default = urlImg;
        },
        setDataPhoto(data){
            this.productDetail = data;
            this.gambar_default = data.gallery[0].photo;
        }
    },
     mounted(){
        axios.get("http://localhost/shayna_backend/public/api/products",{
            params: {
                id: this.$route.params.id
            }
        })
            .then( res => ( this.setDataPhoto(res.data.data) ) )
            .catch( err => console.log(err) );
    }
}
</script>

<style scoped>
    .pt{
        margin-right: 8px;
    }
</style>
