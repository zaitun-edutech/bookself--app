<template>
  <h2>BOOK SELF</h2>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-4">
        <div class="card p-2 shadow">
          <form action="" @submit.prevent="addBook()" class="form-group">
            <div>
              <label for="title">Judul Buku</label>
              <input
                type="text"
                id="title"
                v-model="title"
                class="form-control"
                autocomplete="off"
                required
              />
            </div>
            <div>
              <label for="image">Cover</label>

              <input
                type="text"
                id="image"
                v-model="image"
                class="form-control"
                autocomplete="off"
                required
              />
            </div>

            <button type="submit" class="btn btn-primary mt-3">Add Book</button>
          </form>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-md-6">
          <div class="card p-2 mt-4">
            <h3>Akan di Baca</h3>
            <div v-for="book in toReadBooks" :key="book.id">
              <img :src="book.image" alt="" width="120" />
              <p>{{ book.title }}</p>
              <button
                class="btn btn-info btn-sm mb-2"
                @click="updateStatus(book, 0)"
              >
                Baca buku ini
              </button>
            </div>
          </div>
          <div class="card p-2 mt-4">
            <h3>Sedang di Baca</h3>

            <div v-for="book in readingBooks" :key="book.id">
              <img :src="book.image" alt="" width="120" />
              <p>{{ book.title }}</p>
              <button
                class="btn btn-info btn-smmb-2"
                @click="updateStatus(book, 1)"
              >
                sudah dibaca
              </button>
            </div>
          </div>
          <div class="card p-2 mt-4">
            <h3>Telah di Baca</h3>
            <div v-for="book in haveReadBooks" :key="book.id">
              <img :src="book.image" alt="" width="120" />
              <p>{{ book.title }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref, onMounted, watch } from "vue";
export default {
  name: "App",

  setup() {
    const title = ref("");
    const image = ref("");
    const books = ref([]);

    function addBook() {
      const newBook = {
        id: Date.now,
        title: title.value,
        image: image.value,
        status: -1, // -1 akan dibaca ,0 sedang dan 1 telah
      };

      books.value.push(newBook);
      localStorage.books = JSON.stringify(books.value);
      title.value = "";
      image.value = "";
    }
    function updateStatus(book, status) {
      book.status = status;
      localStorage.books = JSON.stringify(books.value);
    }
    const toReadBooks = computed(() =>
      books.value.filter((book) => book.status == -1)
    );
    const readingBooks = computed(() =>
      books.value.filter((book) => book.status == 0)
    );
    const haveReadBooks = computed(() =>
      books.value.filter((book) => book.status == 1)
    );
    onMounted(() => {
      const BookData = localStorage.books;
      if (BookData) {
        books.value = JSON.parse(BookData);
      }
    });
    watch(readingBooks, (val, oldVal) => {
      if (val.length > 3) {
        alert(
          "sepertinya sudah terlalu banyak membaca buku, istirahat sejenak!"
        );
      }
    });

    return {
      title,
      image,
      addBook,
      books,
      toReadBooks,
      readingBooks,
      haveReadBooks,
      updateStatus,
    };
  },
};
</script>
