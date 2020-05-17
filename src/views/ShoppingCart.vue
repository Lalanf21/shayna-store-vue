<template>
    <div class="ShoppingCart">
        <Header />

        <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row text-left">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Shopping Cart</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th class="p-name text-center">Product Name</th>
                                            <th>Price</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody v-if="keranjang.length > 0">
                                        
                                        <tr v-for="cart in keranjang" :key="cart.id">
                                            <td class="cart-pic first-row">
                                                <img :src="cart.photo" />
                                            </td>
                                            <td class="cart-title first-row text-center">
                                                <h5> {{ cart.name }} </h5>
                                            </td>
                                            <td class="p-price first-row">${{ cart.price }}</td>
                                            <td class="delete-item" @click="removeCart(keranjang.index)">
                                                <a href="#"><i class="material-icons">
                                            close
                                            </i></a></td>
                                        </tr>
                                        
                                    </tbody>
                                    <tbody v-else>
                                        <tr>
                                            <td colspan="4"> Keranjang anda kosong </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-8 text-left">
                            <h4 class="mb-4">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout">
                                <form>
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama" v-model="customerInfo.name">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Email Address</label>
                                        <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email" v-model="customerInfo.email">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP" v-model="customerInfo.number">
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea class="form-control" id="alamatLengkap" rows="3" v-model="customerInfo.address"></textarea>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 text-left">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout">
                                <ul>
                                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                    <li class="subtotal mt-3">Subtotal <span>${{ totalPrice }}</span></li>
                                    <li class="subtotal mt-3">Pajak <span>10% ({{ pajak }})</span></li>
                                    <li class="subtotal mt-3">Total Biaya <span>${{ grandTotal }}</span></li>
                                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                    <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                </ul>
                                <a @click="checkOut()" href="#" class="proceed-btn">I ALREADY PAID</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Shopping Cart Section End -->
    </div>
    
</template>

<script>
import Header from "@/components/Header.vue";
import axios from "axios";

export default {
    name: "ShoppingCart",
    methods: {
        removeCart(index){
            this.keranjang.splice(index,1);
            const parsed = JSON.stringify(this.keranjang);
            localStorage.setItem('keranjang', parsed);
        },
        checkOut(){
            let productId = this.keranjang.map( product => product.id );

            let checkOutData = {
                'name': this.customerInfo.name,
                'email': this.customerInfo.email,
                'number': this.customerInfo.number,
                'address': this.customerInfo.address,
                'transaction_total': this.totalPrice,
                'transaction_status': 'PENDING',
                'transaction_details': productId,
            }

            axios.post("http://localhost/shayna_backend/public/api/checkout", checkOutData)
            .then( this.$router.push('success') )
            .catch( err => console.log(err) );
        }
    },
    components:{
        Header
    },
    data(){
        return{
            keranjang: [],
            customerInfo: {
                name: '',
                email: '',
                number: '',
                address: ''
            }
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
        },
        pajak(){
            return (this.totalPrice * 10) / 100;
        },
        grandTotal(){
            return this.totalPrice + this.pajak;
        }
    }
}
</script>