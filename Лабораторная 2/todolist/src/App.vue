<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light shadow-sm w-100">
    <div class="container-fluid px-4">
      <RouterLink to="/" class="navbar-brand fw-bold">Редактор заметок</RouterLink>

      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <RouterLink class="nav-link" to="/">
              <i class="bi bi-house"></i> Главная
            </RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink class="nav-link" to="/rate">
              <i class="bi bi-star"></i> Оценить
            </RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink class="nav-link" to="/about">
              <i class="bi-person-circle"></i> Об авторе
            </RouterLink>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <main class="flex-grow-1 py-4">
    <div class="container-fluid px-4">
      <RouterView />
    </div>
  </main>

  <footer class="bg-light border-top text-center py-3 mt-auto w-100"></footer>
</template>

<style scoped>
html,
body,
#app {
  height: 100%;
  margin: 0;
}

main {
  background-color: #f8f9fa;
  min-height: calc(100vh - 120px);
}

.navbar {
  position: sticky;
  top: 0;
  z-index: 1000;
}

.nav-link {
  font-size: 1.1rem;
  margin-left: 15px;
}

footer {
  background-color: #f8f9fa;
}
</style>
