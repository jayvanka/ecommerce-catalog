<template>
     <div class="container">
        <div v-if="isLoading" class="card">
            <div class="product-container">
                <div class="skeleton-thumbnail"></div>
                <div class="skeleton-details">
                    <div class="skeleton-details-top"></div>
                    <div class="skeleton-details-bottom"></div>
                </div>
            </div>
        </div>
        <div v-else class="container" :class="!isProductAvailable ? 'bg-gray' : product.data.category === 'men\'s clothing' ? 'bg-blue' : 'bg-pink'">
            <div class="overlay">
                <img src="../assets/bg-pattern.svg" alt="background overlay">
            </div>
            <div class="card">
                <div v-if="!isProductAvailable" class="product-unavailable-container">
                    <div class="overlay">
                        <img src="../assets/sad-face.png" alt="background unavailable product" srcset="">
                    </div>
                    <div class="product-details">
                        <p>This product is unavailable to show</p>
                        <div class="cta">
                            <button type="button" @click="getSingleProduct()" class="cta-next">Next Product</button>
                        </div>
                    </div>
                </div>
                <div v-else class="product-container">
                    <div class="product-thumbnail">
                        <img :src="product.data.image" alt="">
                    </div>
                    <div class="product-details">
                        <div class="top">
                            <h3 :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-darkpink'" class="title">{{ product.data.title }}</h3>
                            <div class="sub-title">
                                <span>{{ product.data.category }}</span>
                                <div class="rating">
                                    <span>{{ product.data.rating.rate }}/5</span>
                                    <div class="rating">
                                        <!-- <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-darkpink'" class="circle"></span> -->
                                        <span v-for="star in ratingStars" :key="star" :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-darkpink'"  class="circle"></span>
                                        <span v-for="star in emptyStars" :key="star"  :class="product.data.category === 'men\'s clothing' ? 'emptycircle-navy' : 'emptycircle-darkpink'" class="emptycircle"></span>
                                    </div>
                                </div>
                            </div>
                            <div class="description">
                                <p>{{ product.data.description }}</p>
                            </div>
                        </div>
                        <div class="bottom">
                            <span :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-darkpink'" class="price">${{ product.data.price }}</span>
                            <div class="cta">
                                <button type="button" :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-darkpink'" class="cta-buy">Buy Now</button>
                                <button type="button" @click="getSingleProduct()" :class="product.data.category === 'men\'s clothing' ? 'border-navy font-navy' : 'border-darkpink font-darkpink'" class="cta-next">Next Product</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductDisplay',
    data() {
        return {
            isLoading: false,
            index: 0,
            isProductAvailable: false,
            product: {},
            rate: 0,
        }
    },
    methods: {
        async callAPI() {
            const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
            const result = await response.json();
            return result;
        },
        async getSingleProduct() {
            this.isLoading = true;

            if (this.index !== 20) {
                this.index++
            } else {
                this.index = 1;
            }

            let data = await this.callAPI()
            if (data.category === "men's clothing" || data.category === "women's clothing") {
                this.product = { data }
                this.isProductAvailable = true;
            } else {
                this.isProductAvailable = false;
            }

            this.isLoading = false;
        }
    },
    mounted() {
        this.getSingleProduct();
    },
    computed: {
    ratingStars() {
      const rating = Math.round(this.product.data.rating.rate); // Bulatkan rating ke bilangan bulat terdekat
      return Array.from({ length: rating }, () => null);
    },
    emptyStars() {
      const emptyrating = Math.round(5 - this.product.data.rating.rate);
      return Array.from({ length: emptyrating }, () => null);
    },
  },
}
</script>

<style scoped>
    @import '../assets/style/page.css'
</style>
