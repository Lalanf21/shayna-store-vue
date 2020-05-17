<template>
     <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :dots="false" :nav="false" :autoplay="true">
                        <div class="product-item" v-for="itemProduct in products" :key="itemProduct.id">
                            <div class="pi-pic">
                                <img :src="itemProduct.gallery[0].photo" alt="" />
                                <ul>
                                    <li class="w-icon active" @click="saveKeranjang(itemProduct.id, itemProduct.name,itemProduct.gallery[0].photo,itemProduct.price)">
                                        <a href="#">
                                            <i class="icon_bag_alt"></i>
                                        </a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link :to="'/product/'+itemProduct.id"> + Quick View </router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type }}</div>
                                <router-link :to="'/product/'+itemProduct.id">
                                    <h5>{{ itemProduct.name }}</h5>
                                </router-link>
                                <div class="product-price">
                                    Rp.{{ itemProduct.price }}
                                    <!-- <span>200</span> -->
                                </div>
                            </div>
                        </div>
                    </carousel>
                </div>
                <div class="col-lg-12" v-else>
                    <p>
                        Barang terbaru belum tersedia
                    </p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name: 'WomanProduct',
    components:{
        carousel
    },
    data(){
        return{
            products: [],
            keranjang: [],
        }
    },
    methods: {
        removeCart(index){
            this.keranjang.splice(index,1);
            const parsed = JSON.stringify(this.keranjang);
            localStorage.setItem('keranjang', parsed);
        },
        saveKeranjang(idProduct,name,photo,price){
            
            let productStored = {
                "id": idProduct,
                "name": name,
                "photo": photo,
                "price": price,
            }

            this.keranjang.push(productStored);
            const parsed = JSON.stringify(this.keranjang);
            localStorage.setItem('keranjang', parsed);
        }
    },
    mounted(){
        if (localStorage.getItem('keranjang')) {
            try {
                this.keranjang = JSON.parse(localStorage.getItem('keranjang'));
            } catch(e) {
                localStorage.removeItem('keranjang');
            }
        }
        axios.get("http://localhost/shayna_backend/public/api/products")
            .then( res => (this.products = res.data.data.data))
            .catch( err => console.log(err) );
    }
}
</script>

<style scoped>
    .product-item{
        margin-right: 25px;
    }
</style>