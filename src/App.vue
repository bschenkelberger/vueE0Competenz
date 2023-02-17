<template>
  <div id="app">
    <div class="header">
      <span class="brand">Vue Library</span>
      <span class="add-new-book-button">
        <a href="#" @click="openNewBookModal">Add a book</a>
      </span>
      <div class="books-count" title="Your Bookings">{{ userBooksCount }}</div>
    </div>
    <div class="container">
      <NewBook v-show="showNewBookModal" @send-new-book="addNewBook" @close-book-modal="closeNewBookModal"/>
      <Book v-for="(book, index) in currentBooks" :book="book" :key="index"></Book>
    </div>
  </div>
</template>

<script>
import Book from './components/Book'
import NewBook from './components/NewBook'

export default {
  name: 'App',
  components: { NewBook, Book },
  data() {
    return {
      currentBooks: [
        {
          name: 'Harry Potter',
          overview: '',
          volumes:  [
                      { name: '' }
                    ]
        }
      ],
      showNewBookModal: false,
    };
  },
  methods: {
    openNewBookModal() {
      this.showNewBookModal = true;
    },
    closeNewBookModal() {
      this.showNewBookModal = false;
    },
    addNewBook(newBookDetails) {
      if(this.isUniqueBook(newBookDetails)) {
        this.currentBooks.push(newBookDetails);
        console.log('addNewBook-currentBooks:' , this.currentBooks);
        alert('Book added successfully.');
      }
      else
        alert('Book name/overview already exists.');
    },
    isUniqueBook(newBookDetails) {
      console.log('isUniqueBook:' , newBookDetails);
      //Bug im Testcase
//      if(newBookDetails.name == 'Harry Potter' && newBookDetails.overview == undefined) {
//        return false;
//      }
      
      for(var i = 0;i < this.currentBooks.length;i++) {
        console.log('isUniqueBook-currentBooks:' , this.currentBooks[i]);
        if( newBookDetails.name != undefined && this.currentBooks[i].name.toUpperCase() == newBookDetails.name.toUpperCase()) {
          return false;
        }
        if( newBookDetails.overview != undefined && this.currentBooks[i].overview.toUpperCase() == newBookDetails.overview.toUpperCase()) {
          return false;
        }

        
      }
      return true;
    }
  },
  computed: {
    userBooksCount() {
      return this.currentBooks.length;
    }
  }
}
</script>

<style>
body {
  margin: 0px;
}

.header {
  padding: 10px;
  background: linear-gradient(to right, #B5F595, #DEFCCF);
}

.books-count {
  width: 25%;
  float: right;
  text-align: right;
  padding-right: 2%;
  padding-top: 0.8%;
}

.brand {
  font-weight: bold;
  font-size: 1.8em;
}

.container {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;
}

.add-new-book-button {
  padding-left: 15px;
}
</style>