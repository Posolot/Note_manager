<template>
  <div id="app" class="container mt-4">
    <h1 class="mb-4 text-center">Note Manager</h1>

    <!-- Форма для создания заметок -->
    <div class="card p-3 mb-4 shadow-sm">
      <h3>Create a Note</h3>
      <div class="mb-3">
        <label for="title" class="form-label">Title</label>
        <input type="text" id="title" v-model="newNote.title" class="form-control" placeholder="Enter note title" />
      </div>
      <div class="mb-3">
        <label for="text" class="form-label">Text</label>
        <textarea
          id="text"
          v-model="newNote.text"
          class="form-control"
          rows="3"
          placeholder="Enter note text"
        ></textarea>
      </div>
      <div class="mb-3">
        <label class="form-label">Category</label>
        <div>
          <input type="radio" id="work" value="Work" v-model="newNote.category" />
          <label for="work" class="me-3">Work</label>
          <input type="radio" id="personal" value="Personal" v-model="newNote.category" />
          <label for="personal">Personal</label>
        </div>
      </div>
      <div class="mb-3 form-check">
        <input type="checkbox" id="important" v-model="newNote.important" class="form-check-input" />
        <label for="important" class="form-check-label">Mark as important</label>
      </div>
      <button class="btn btn-primary w-100" @click="addNote">Add Note</button>
    </div>

    <!-- Список заметок с фильтрацией -->
    <div class="mb-3">
      <label for="filter" class="form-label">Filter by Category</label>
      <select id="filter" v-model="filter" class="form-select">
        <option value="All">All</option>
        <option value="Work">Work</option>
        <option value="Personal">Personal</option>
      </select>
    </div>

    <div v-if="filteredNotes.length > 0">
      <div v-for="(note, index) in filteredNotes" :key="index" class="card mb-3 shadow-sm note-card">
        <div class="card-body">
          <h5 class="card-title d-flex justify-content-between align-items-center">
            {{ note.title }}
            <span v-if="note.important" class="badge bg-warning text-dark">Important</span>
          </h5>
          <p class="card-text">{{ note.text }}</p>
          <p><strong>Category:</strong> {{ note.category }}</p>
          <button class="btn btn-danger btn-sm w-100" @click="deleteNote(index)">Delete</button>
        </div>
      </div>
    </div>
    <p v-else class="text-center text-muted">No notes available.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newNote: {
        title: '',
        text: '',
        category: 'Work',
        important: false
      },
      notes: [],
      filter: 'All'
    };
  },
  computed: {
    filteredNotes() {
      if (this.filter === 'All') {
        return this.notes;
      }
      return this.notes.filter(note => note.category === this.filter);
    }
  },
  methods: {
    addNote() {
      if (this.newNote.title.trim() && this.newNote.text.trim()) {
        this.notes.push({ ...this.newNote });
        this.resetNewNote();
      } else {
        alert('Please fill in all fields.');
      }
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
    },
    resetNewNote() {
      this.newNote = {
        title: '',
        text: '',
        category: 'Work',
        important: false
      };
    }
  }
};
</script>

<style scoped>
/* Общие стили */
body {
  background-color: #f8f9fa;
  font-family: 'Arial', sans-serif;
}

/* Карточка заметок */
.note-card {
  border-left: 5px solid #0d6efd;
  transition: transform 0.2s ease-in-out;
}

.note-card:hover {
  transform: scale(1.02);
}

/* Кнопка добавления */
button {
  font-size: 1rem;
}

/* Пустой список */
.text-muted {
  font-size: 1.2rem;
}

/* Заголовок */
h1 {
  font-size: 2.5rem;
  color: #333;
}

.card-title {
  font-weight: bold;
}
</style>

