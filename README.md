# 📚 Book API – Node.js + Express

A simple REST API to manage a list of books. Built with Node.js and Express, storing data in-memory (no database).

## 🚀 Features
- Retrieve all books (GET)
- Add a new book (POST)
- Update an existing book (PUT)
- Delete a book (DELETE)

## 🛠 Tools
- Node.js
- Express
- Postman for testing

## 📦 Installation

```bash
git clone https://github.com/your-username/book-api.git
cd book-api
npm install
npm start

package.json
{
  "name": "book-api",
  "version": "1.0.0",
  "description": "A simple REST API to manage a list of books using Node.js and Express.",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "author": "Your Name",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.4"
  }
}

index.js
const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json());

let books = [];
let idCounter = 1;

// GET /books - Retrieve all books
app.get('/books', (req, res) => {
  res.json(books);
});

// POST /books - Add a new book
app.post('/books', (req, res) => {
  const { title, author } = req.body;
  if (!title || !author) {
    return res.status(400).json({ message: "Title and author are required." });
  }
  const newBook = { id: idCounter++, title, author };
  books.push(newBook);
  res.status(201).json(newBook);
});

// PUT /books/:id - Update a book by ID
app.put('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const { title, author } = req.body;
  const book = books.find(b => b.id === bookId);

  if (!book) {
    return res.status(404).json({ message: "Book not found." });
  }

  book.title = title || book.title;
  book.author = author || book.author;
  res.json(book);
});

// DELETE /books/:id - Remove a book by ID
app.delete('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const initialLength = books.length;
  books = books.filter(b => b.id !== bookId);

  if (books.length === initialLength) {
    return res.status(404).json({ message: "Book not found." });
  }

  res.json({ message: "Book deleted successfully." });
});

// Start the server
app.listen(PORT, () => {
  console.log(`📚 Book API running at http://localhost:${PORT}`);
});
