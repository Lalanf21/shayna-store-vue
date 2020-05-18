<template>
    <header class="header-section">
        <div class="header-top">
            <div class="container">
                <div class="ht-left">
                    <div class="mail-service">
                        <i class=" fa fa-envelope"></i> hello.shayna@gmail.com
                    </div>
                    <div class="phone-service">
                        <i class=" fa fa-phone"></i> +628 22081996
                    </div>
                </div>
            </div>
        </div>
        <div class="container">
            <div class="inner-header">
                <div class="row">
                    <div class="col-lg-2 col-md-2">
                        <div class="logo">
                            <router-link to="/">
                                <img src="img/logo_website_shayna.png"/>
                            </router-link>
                        </div>
                    </div>
                    <div class="col-lg-7 col-md-7"></div>
                    <div class="col-lg-3 text-right col-md-3">
                        <ul class="nav-right">
                            <li class="cart-icon">
                                Keranjang Belanja &nbsp;
                                <a href="#">
                                    <i class="icon_bag_alt"></i>
                                    <span> {{ keranjang.length }} </span>
                                </a>
                                <div class="cart-hover">
                                    <div class="select-items">
                                        <table>
                                            <tbody v-if="keranjang.length > 0">

                                                <tr v-for="cart in keranjang" :key="cart.id" >
                                                    <td class="si-pic">
                                                        <img :src="cart.photo"/>
                                                    </td>
                                                    <td class="si-text">
                                                        <div class="product-selected">
                                                            <p>${{ cart.price }}</p>
                                                            <h6>{{ cart.name }}</h6>
                                                        </div>
                                                    </td>
                                                    <td class="si-close" @click="removeCart(cart.id)">
                                                        <i class="ti-close"></i>
                                                    </td>
                                                </tr>
                                            </tbody>
                                            <tbody v-else>
                                                <tr>
                                                    <td>keranjang anda kosong</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <div class="select-total">
                                        <span>total:</span>
                                        <h5>${{ totalPrice }}</h5>
                                    </div>
                                    <div class="select-button">
                                        <router-link to="/cart" class="primary-btn view-card">
                                            VIEW CART
                                        </router-link>
                                        <!-- <a href="#" class="primary-btn view-card">VIEW CARD</a> -->
                                        <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>

<script>
export default {
    name: 'Header',
    methods: {
        removeCart(idx){
            // this.keranjang.splice(index,1);
            // const parsed = JSON.stringify(this.keranjang);
            // localStorage.setItem('keranjang', parsed);
            let keranjangStorage = JSON.parse(localStorage.getItem('keranjang'));
            let itemKeranjangStorage = keranjangStorage.map(itemKeranjangStorage => itemKeranjangStorage.id);
            let index = itemKeranjangStorage.findIndex(id=>id == idx);
            this.keranjang.splice(index,1);

            const parsed = JSON.stringify(this.keranjang);
            localStorage.setItem('keranjang',parsed);
            window.location.reload();
        }
    },
    data(){
        return {
            keranjang: []
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
    },
    computed: {
        totalPrice(){
            return this.keranjang.reduce( (items,data) => items + data.price,0);
        }
    }
}
</script>

<style scoped>
    .si-pic{
        width: 75px;
    }
</style>