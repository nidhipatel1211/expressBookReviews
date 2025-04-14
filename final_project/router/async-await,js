const express = require('express');
let books = require("./booksdb.js");
let isValid = require("./auth_users.js").isValid;
let users = require("./auth_users.js").users;
const public_users = express.Router();

// Simulating asynchronous operation for validation and user registration
const asyncIsValid = (username) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(isValid(username));
    }, 100);
  });
};

const asyncRegisterUser = (username, password) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      users.push({ username, password, currDate: new Date() });
      resolve();
    }, 100);
  });
};

public_users.post("/register", async (req, res) => {
  let username = req.body.username;
  let password = req.body.password;

  if (!username || !password) {
    return res.status(400).json({ "message": "username and password are required." });
  }

  if (!await asyncIsValid(username)) {
    await asyncRegisterUser(username, password);
    return res.status(201).json({ "message": "User registered successfully" });
  } else {
    return res.status(404).json({ message: "username already exists" });
  }
});

// Simulating asynchronous operations for books
const asyncGetBooks = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(books);
    }, 100);
  });
};

const asyncGetBookByISBN = (isbn) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(books[isbn]);
    }, 100);
  });
};

const asyncGetBooksByAuthor = (author) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(Object.values(books).filter(book => book.author === author));
    }, 100);
  });
};

const asyncGetBooksByTitle = (title) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(Object.values(books).filter(book => book.title === title));
    }, 100);
  });
};

const asyncGetBookReviews = (isbn) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(books[isbn]?.reviews);
    }, 100);
  });
};

// Get the book list available in the shop
public_users.get('/', async (req, res) => {
  const booksList = await asyncGetBooks();
  return res.status(200).json(booksList);
});

// Get book details based on ISBN
public_users.get('/isbn/:isbn', async (req, res) => {
  let isbn = req.params.isbn;
  const book = await asyncGetBookByISBN(isbn);
  if (!book) {
    return res.status(404).json({ message: "Book not found" });
  }
  return res.status(200).json(book);
});

// Get book details based on author
public_users.get('/author/:author', async (req, res) => {
  const author = req.params.author;
  const filteredBooks = await asyncGetBooksByAuthor(author);
  if (filteredBooks.length > 0) {
    return res.status(200).json(filteredBooks);
  } else {
    return res.status(404).json({ message: "No books found for this author" });
  }
});

// Get all books based on title
public_users.get('/title/:title', async (req, res) => {
  const title = req.params.title;
  const filteredBooks = await asyncGetBooksByTitle(title);
  if (filteredBooks.length > 0) {
    return res.status(200).json(filteredBooks);
  } else {
    return res.status(404).json({ message: "No books found for this title" });
  }
});

// Get book review
public_users.get('/review/:isbn', async (req, res) => {
  let isbn = req.params.isbn;
  const reviews = await asyncGetBookReviews(isbn);
  if (reviews) {
    return res.status(200).json(reviews);
  } else {
    return res.status(404).json({ message: "No reviews found for this book" });
  }
});

module.exports.asyncGeneral = public_users;