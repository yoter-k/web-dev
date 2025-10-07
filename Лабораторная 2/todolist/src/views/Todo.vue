<script setup>
import Swal from "sweetalert2";
import { ref } from "vue";

const notes = ref([]);

const activeNote = ref(null);

const editingId = ref(null);
const editTitle = ref("");

const editingTodoId = ref(null);
const editTodoText = ref("");

const addNote = () => {
  const newNote = {
    id: Date.now(),
    title: "Новая заметка",
    todos: [],
  };
  notes.value.push(newNote);
  activeNote.value = newNote;
};

const startEdit = (note) => {
  editingId.value = note.id;
  editTitle.value = note.title;
};

const confirmEdit = () => {
  const note = notes.value.find((n) => n.id === editingId.value);
  if (note && editTitle.value.trim()) {
    note.title = editTitle.value;
  }
  editingId.value = null;
};

const cancelEdit = () => {
  editingId.value = null;
};

const deleteNote = (note) => {
  Swal.fire({
    title: "Удалить заметку?",
    text: note.title,
    icon: "warning",
    showCancelButton: true,
    confirmButtonText: "Да, удалить",
    cancelButtonText: "Отмена",
  }).then((result) => {
    if (result.isConfirmed) {
      notes.value = notes.value.filter((n) => n.id !== note.id);
      if (activeNote.value?.id === note.id) activeNote.value = null;
    }
  });
};

const addTodo = () => {
  if (!activeNote.value) return;
  activeNote.value.todos.push({
    id: Date.now(),
    text: "Новое дело",
    done: false,
  });
};

const startEditTodo = (todo) => {
  editingTodoId.value = todo.id;
  editTodoText.value = todo.text;
};

const confirmEditTodo = () => {
  const todo = activeNote.value.todos.find((t) => t.id === editingTodoId.value);
  if (todo && editTodoText.value.trim()) {
    todo.text = editTodoText.value;
  }
  editingTodoId.value = null;
};

const cancelEditTodo = () => {
  editingTodoId.value = null;
};

const deleteTodo = (todo) => {
  activeNote.value.todos = activeNote.value.todos.filter((t) => t.id !== todo.id);
};
</script>

<template>
  <div class="container py-4">
    <div class="row">
      <div class="col-4">
        <h3 class="mb-4">Мои списки дел</h3>

        <ul class="list-group mb-3">
          <li
            v-if="notes.length === 0"
            class="list-group-item bg-warning text-center custom-height"
          >
            Нет заметок...
          </li>
          <li
            v-else
            v-for="note in notes"
            :key="note.id"
            class="list-group-item d-flex justify-content-between align-items-center"
            :class="{ 'active text-white': activeNote && activeNote.id === note.id }"
            @click="activeNote = note"
            style="cursor: pointer"
          >
            <div class="d-flex align-items-center flex-grow-1">
              <span v-if="editingId !== note.id">{{ note.title }}</span>
              <input
                v-else
                v-model="editTitle"
                placeholder="Название заметки"
                class="form-control"
              />
            </div>
            <div>
              <template v-if="editingId === note.id">
                <button @click.stop="confirmEdit" class="btn btn-sm btn-success me-1">
                  <i class="bi bi-check"></i>
                </button>
                <button @click.stop="cancelEdit" class="btn btn-sm btn-secondary">
                  <i class="bi bi-x"></i>
                </button>
              </template>
              <template v-else>
                <button @click.stop="startEdit(note)" class="btn btn-sm btn-warning me-1">
                  <i class="bi bi-pencil"></i>
                </button>
                <button @click.stop="deleteNote(note)" class="btn btn-sm btn-danger">
                  <i class="bi bi-trash"></i>
                </button>
              </template>
            </div>
          </li>
        </ul>

        <button @click="addNote" class="btn btn-primary w-100">
          <i class="bi bi-plus-circle"></i> Добавить заметку
        </button>
      </div>

      <div class="col-8">
        <h3 class="mb-4">Содержание заметки</h3>

        <div v-if="!activeNote" class="alert alert-info text-center custom-height">
          Выберите заметку...
        </div>
        <template v-else>
          <ul class="list-group mb-3">
            <li v-if="activeNote.todos.length === 0" class="list-group-item text-center">
              Нет пунктов...
            </li>
            <li
              v-else
              v-for="todo in activeNote.todos"
              :key="todo.id"
              class="list-group-item d-flex justify-content-between align-items-center"
            >
              <div class="d-flex align-items-center flex-grow-1">
                <input
                  type="checkbox"
                  v-model="todo.done"
                  class="form-check-input me-2"
                />
                <template v-if="editingTodoId === todo.id">
                  <input
                    v-model="editTodoText"
                    placeholder="Текст задачи"
                    class="form-control mx-2"
                  />
                </template>
                <span v-else>{{ todo.text }}</span>
              </div>
              <div>
                <template v-if="editingTodoId === todo.id">
                  <button @click="confirmEditTodo" class="btn btn-sm btn-success me-1">
                    <i class="bi bi-check"></i>
                  </button>
                  <button @click="cancelEditTodo" class="btn btn-sm btn-secondary">
                    <i class="bi bi-x"></i>
                  </button>
                </template>
                <template v-else>
                  <button
                    @click="startEditTodo(todo)"
                    class="btn btn-sm btn-warning me-1"
                  >
                    <i class="bi bi-pencil"></i>
                  </button>
                  <button class="btn btn-sm btn-outline-danger" @click="deleteTodo(todo)">
                    <i class="bi bi-x-circle"></i>
                  </button>
                </template>
              </div>
            </li>
          </ul>

          <button class="btn btn-primary w-100" @click="addTodo">
            <i class="bi bi-plus-lg"></i> Добавить новый пункт
          </button>
        </template>
      </div>
    </div>
  </div>
</template>

<style scoped>
.custom-height {
  min-height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
