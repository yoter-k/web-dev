<script setup>
import { ref, onMounted } from 'vue'

const CART_KEY = 'cart'
const cart = ref([])

function loadCart() {
    const stored = localStorage.getItem(CART_KEY)
    cart.value = stored ? JSON.parse(stored) : []
}

function saveCart() {
    localStorage.setItem(CART_KEY, JSON.stringify(cart.value))
}

function removeItem(id) {
    cart.value = cart.value.filter(item => item.id !== id)
    saveCart()
}

function increaseQuantity(id) {
    const item = cart.value.find(p => p.id === id)
    if (item) {
        item.quantity += 1
        saveCart()
    }
}

function decreaseQuantity(id) {
    const item = cart.value.find(p => p.id === id)
    if (item && item.quantity > 1) {
        item.quantity -= 1
    } else {
        removeItem(id)
    }
    saveCart()
}

onMounted(loadCart)
</script>

<template>
    <main class="container my-5">
        <h1 class="mb-4 text-center">Корзина</h1>

        <div v-if="!cart.length" class="text-center">
            <p>Корзина пуста</p>
            <RouterLink to="/products-table" class="btn btn-primary">Вернуться к товарам</RouterLink>
        </div>

        <div v-else>
            <table class="table table-striped align-middle">
                <thead>
                    <tr>
                        <th>Фото</th>
                        <th>Название</th>
                        <th>Цена</th>
                        <th>Количество</th>
                        <th>Действия</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in cart" :key="item.id">
                        <td><img :src="item.image" width="60" /></td>
                        <td>{{ item.name }}</td>
                        <td>{{ item.price }}</td>
                        <td>{{ item.quantity }}</td>
                        <td>
                            <button class="btn btn-sm btn-success me-2" @click="increaseQuantity(item.id)">+</button>
                            <button class="btn btn-sm btn-warning me-2" @click="decreaseQuantity(item.id)">−</button>
                            <button class="btn btn-sm btn-danger" @click="removeItem(item.id)">Удалить</button>
                        </td>
                    </tr>
                </tbody>
            </table>

            <div class="text-end fw-bold fs-5 mt-3">
                Итого: {{
                    cart.reduce((sum, item) =>
                        sum + parseInt(item.price.replace(/\D/g, '')) * item.quantity, 0
                    ).toLocaleString()
                }} р.
            </div>
        </div>
    </main>
</template>
