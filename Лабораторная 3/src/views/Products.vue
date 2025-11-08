<script setup>
import { ref, onMounted } from 'vue'

const STORAGE_KEY = 'products'
const CART_KEY = 'cart'
const products = ref([])

const INITIAL_PRODUCTS = [
  { id: 1, name: 'MacBook Air 13"', description: 'Лёгкий и производительный ноутбук с процессором Apple M2 и Retina дисплеем.', price: '120 000 р.', image: '/assets/img/t1.jpg' },
  { id: 2, name: 'Клавиатура Logitech G915', description: 'Беспроводная механическая клавиатура с RGB-подсветкой и низким откликом.', price: '18 000 р.', image: '/assets/img/t2.jpg' },
  { id: 3, name: 'Наушники Sony WH-1000XM5', description: 'Беспроводные наушники с активным шумоподавлением и высоким качеством звука.', price: '25 000 р.', image: '/assets/img/t3.jpg' }
]

function loadProducts() {
  const stored = localStorage.getItem(STORAGE_KEY)
  if (stored) {
    products.value = JSON.parse(stored)
  } else {
    products.value = INITIAL_PRODUCTS
    localStorage.setItem(STORAGE_KEY, JSON.stringify(INITIAL_PRODUCTS))
  }
}

onMounted(loadProducts)

function addToCart(product) {
  const storedCart = localStorage.getItem(CART_KEY)
  const cart = storedCart ? JSON.parse(storedCart) : []

  const existing = cart.find(p => p.id === product.id)
  if (existing) {
    existing.quantity += 1
  } else {
    cart.push({ ...product, quantity: 1 })
  }

  localStorage.setItem(CART_KEY, JSON.stringify(cart))
  alert(`Товар "${product.name}" добавлен в корзину`)
}
</script>

<template>
  <main class="container my-5">
    <h1 class="mb-4 text-center">Наши товары</h1>

    <div class="text-center mb-5">
      <RouterLink to="/products-table" class="btn btn-primary">Список товаров (таблица)</RouterLink>
      <RouterLink to="/cart" class="btn btn-primary ms-2">Перейти в корзину</RouterLink>
    </div>

    <div v-for="product in products.slice(0, 3)" :key="product.id" class="card shadow rounded my-3">
      <div class="card-body">
        <div class="row">
          <div class="col-md-2">
            <img :src="product.image" class="product-image" :alt="product.name" />
          </div>
          <div class="col-md-10 d-flex flex-column">
            <div class="product-name">{{ product.name }}</div>
            <div class="product-description">{{ product.description }}</div>
            <div class="flex-grow-1"></div>
            <div class="d-flex justify-content-end align-items-center">
              <div class="product-price me-3">Цена: {{ product.price }}</div>
              <button class="btn btn-warning" @click="addToCart(product)">
                <i class="bi bi-cart"></i> В корзину
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.product-image {
  width: 100%;
  max-width: 120px;
  object-fit: cover;
}

.product-name {
  font-weight: bold;
  font-size: 1.1rem;
}

.product-description {
  font-size: 0.95rem;
  margin-bottom: 10px;
}
</style>
