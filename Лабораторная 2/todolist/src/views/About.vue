<script setup></script>

<template>
  <div class="container py-4">
    <h1>Об авторе</h1>
    <p>
      Краткое описание о себе: Я Чуменко Игорь, студент группы ПИНБ-31, обучаюсь по
      специальности программная инженерия. Увлекаюсь веб-разработкой и стремлюсь развивать
      свои навыки в создании современных веб-приложений.😊
    </p>

    <div class="row justify-content-center">
      <div class="col-md-4">
        <div class="card shadow rounded my-3">
          <div class="card-body text-center">
            <div><img src="../assets/img/author.jpg" class="avatar" /></div>
            <div class="avatar-post">Автор</div>
            <div class="avatar-text">Игорь, студент</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.avatar {
  border-radius: 50%;
  width: 250px;
  height: 250px;
  object-fit: cover;
}

.avatar-post {
  margin-top: 1rem;
  font-size: 1.4rem;
  text-align: center;
}

.avatar-text {
  color: rgb(140, 140, 140);
  font-size: 1.1rem;
  text-align: center;
}
</style>
