<script setup>
import { ref, computed, onMounted } from 'vue'

const STORAGE_KEY = 'products'
const CART_KEY = 'cart'
const perPage = 3
const currentPage = ref(1)
const products = ref([])

async function loadProducts() {
    const stored = localStorage.getItem(STORAGE_KEY)

    if (stored) {
        products.value = JSON.parse(stored)
    } else {
        const response = await fetch('/data/products.json')
        if (!response.ok) {
            console.error('Ошибка загрузки products.json')
            return
        }
        const data = await response.json()
        products.value = data
        localStorage.setItem(STORAGE_KEY, JSON.stringify(data))
    }
}

onMounted(loadProducts)
window.addEventListener('storage', loadProducts)

const totalPages = computed(() => Math.ceil(products.value.length / perPage))
const paginatedProducts = computed(() => {
    const start = (currentPage.value - 1) * perPage
    return products.value.slice(start, start + perPage)
})

function goToPage(page) {
    if (page < 1 || page > totalPages.value) return
    currentPage.value = page
}

function addToCart(product) {
    const storedCart = localStorage.getItem(CART_KEY)
    const cart = storedCart ? JSON.parse(storedCart) : []

    const existing = cart.find(p => p.id === product.id)
    if (existing) {
        existing.quantity = (existing.quantity || 1) + 1
    } else {
        cart.push({ ...product, quantity: 1 })
    }

    localStorage.setItem(CART_KEY, JSON.stringify(cart))
    alert(`Товар "${product.name}" добавлен в корзину`)
}
</script>

<template>
    <main class="container my-5">
        <h1>Список товаров</h1>

        <div class="mb-3 text-end">
            <RouterLink to="/products-editor" class="btn btn-primary" style="margin-right: 5px;">Редактировать товары
            </RouterLink>
            <RouterLink to="/cart" class="btn btn-primary">Корзина</RouterLink>
        </div>

        <div class="table-responsive">
            <table class="table table-hover align-middle">
                <thead>
                    <tr>
                        <th>Фото</th>
                        <th>Название</th>
                        <th>Описание</th>
                        <th>Цена</th>
                        <th>Действие</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="product in paginatedProducts" :key="product.id">
                        <td><img :src="product.image" :alt="product.name" width="60" /></td>
                        <td>{{ product.name }}</td>
                        <td>{{ product.description }}</td>
                        <td>{{ product.price }}</td>
                        <td>
                            <button class="btn btn-warning btn-sm" @click="addToCart(product)">
                                <i class="bi bi-cart"></i> В корзину
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <nav v-if="totalPages > 1" aria-label="Page navigation">
            <ul class="pagination justify-content-center">
                <li class="page-item" :class="{ disabled: currentPage === 1 }">
                    <button class="page-link" @click="goToPage(currentPage - 1)">Назад</button>
                </li>
                <li class="page-item" v-for="page in totalPages" :key="page" :class="{ active: currentPage === page }">
                    <button class="page-link" @click="goToPage(page)">{{ page }}</button>
                </li>
                <li class="page-item" :class="{ disabled: currentPage === totalPages }">
                    <button class="page-link" @click="goToPage(currentPage + 1)">Вперед</button>
                </li>
            </ul>
        </nav>
    </main>
</template>

<style scoped>
.table td,
.table th {
    vertical-align: middle;
}
</style>
