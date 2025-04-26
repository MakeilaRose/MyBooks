<script setup lang="ts">
import { ref } from 'vue'
import { gapi } from 'gapi-script'

const query = ref('')
const books = ref<any[]>([])

async function searchBooks() {
  if (!query.value) return

  const response = await gapi.client.books.volumes.list({
    q: query.value,
    maxResults: 40,
  })

  books.value = response.result.items || []
}
</script>

<template>
  <div class="search-container">
    <div class="search">
      <input
        type="text"
        v-model="query"
        @keydown.enter="searchBooks"
        placeholder="Search for books..."
      />
      <button @click="searchBooks">Search</button>
    </div>

    <div class="books">
      <div v-for="book in books" :key="book.id" class="book-card">
        <img
          v-if="book.volumeInfo.imageLinks"
          :src="book.volumeInfo.imageLinks.thumbnail"
          alt="Book Cover"
        />
        <h3>{{ book.volumeInfo.title }}</h3>
        <p v-if="book.volumeInfo.authors">
          By: {{ book.volumeInfo.authors.join(', ') }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.search-container {
  text-align: center;
  padding: 20px;
}
.search {
  margin: 20px 0;
}
input[type="text"] {
  padding: 8px;
  width: 300px;
  font-size: 16px;
}
button {
  padding: 8px 12px;
  margin-left: 8px;
  cursor: pointer;
}
.books {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
.book-card {
  width: 150px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
}
.book-card img {
  width: 100%;
  height: auto;
  border-radius: 4px;
}
h3 {
  font-size: 16px;
  margin: 8px 0 4px;
}
p {
  font-size: 14px;
  color: #555;
}
</style>
