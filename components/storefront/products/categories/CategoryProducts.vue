<template>
	<section class="text-gray-600 body-font">
		<div class="grid gap-4 mb-4 grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
			<div 
				v-for="(categoryProduct, index) in categoryProducts" :key="index"
				class="relative w-full">
				<div class="relative group hover:scale-105 transform transition duration-200 my-6">
					<NuxtLink :to="{ 
						name: 'category-slug', 
						params: { 
							category: categoryProduct.category_slug, 
							slug: categoryProduct.slug 
						}}">
						<img 
							:alt="categoryProduct.description" 
							class="h-full rounded w-full object-cover object-center" 
							:src="categoryProduct.image">
					</NuxtLink>
						<div class="opacity-0 w-full group-hover:opacity-100 duration-300 absolute bottom-0 right-0 z-10">
						<div class="flex justify-end m-2">
							<button class="text-black mr-2">
							<Heart class="h-6 w-6" />
							</button>
							<button class="text-black">
							<Eye class="h-6 w-6" />
							</button>
						</div>
						<button 
							@click="addProductToCart(categoryProduct, 1)" 
							class="text-white bg-black hover:bg-gray-900 text-center py-4 font-semibold w-full">
							Add to Cart
						
						</button>
						</div>
					</div>
					<NuxtLink :to="{ 
						name: 'category-slug', 
						params: { 
							category: categoryProduct.category_slug, 
							slug: categoryProduct.slug 
						}}"
						class="mb-4">
						<h1 class="text-black text-2xl mb-1">{{categoryProduct.name}}</h1>
						<p class="mt-1 text-gray-900">${{categoryProduct.price}}</p>
					</NuxtLink>
			</div>
		</div>
	</section>
</template>

<script lang="ts">
import { Prop, Component, Vue } from 'nuxt-property-decorator'
import Heart from '@/assets/icons/Heart.vue'
import Eye from '@/assets/icons/Eye.vue'
import { ProductSeed } from '@/types/products/Product'
import { CartItem } from '@/types/cart/Cart'

@Component({
	components: { Heart, Eye }
})
export default class categoryProducts extends Vue {

	@Prop({ required: true }) readonly categoryProducts!: ProductSeed[]
	public localData: CartItem = {
		product_id: null,
		price: null,
		name: null,
		image: null,
		quantity: null
	}
	public items!: []

	public addProductToCart(categoryProduct: ProductSeed, quantity: number): void {
		this.localData = {
			product_id: categoryProduct.id,
			price: categoryProduct.price,
			name: categoryProduct.name,
			image: categoryProduct.image,
			quantity: quantity
		}
		this.$accessor.cart.addToCart(this.localData)
	}

}
</script>
