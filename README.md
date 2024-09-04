"We made a library management systme with the help of HTML, CSS and Javascript . We Used open google API to fetch books from API. make a documentation file" 

Biography.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Biography Books</title>
    <link rel = "stylesheet" href="style.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: auto;
            overflow: hidden;
            padding-top: 100px;
        }

        #books {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .book {
            background: white;
            border: 1px solid #ddd;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .book img {
            max-width: 100%;
            height: auto;
        }

        .book h3 {
            font-size: 18px;
            margin: 10px 0;
        }

        .book p {
            font-size: 14px;
            color: #666;
        }

        .search-bar {
            text-align: center;
            margin: 20px 0;
        }

        .search-bar input {
            padding: 10px;
            font-size: 16px;
            width: 300px;
            margin-right: 10px;
        }

        .search-bar button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Reference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <div class="container">
            <h1>Biography Books</h1>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let page = 1;
            const booksContainer = document.getElementById('books');
            let isLoading = false;
            const seenBooks = new Set(); // To track seen book IDs

            // Shuffle the books array to randomize positions
            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            // Fetch a fallback book image from Unsplash
            function fetchFallbackImage() {
                const UNSPLASH_API_URL = `https://api.unsplash.com/search/photos?query=book&client_id=YOUR_UNSPLASH_ACCESS_KEY`;

                return fetch(UNSPLASH_API_URL)
                    .then(response => response.json())
                    .then(data => {
                        if (data.results.length > 0) {
                            return data.results[0].urls.small;
                        } else {
                            return 'https://via.placeholder.com/150';
                        }
                    })
                    .catch(() => 'https://via.placeholder.com/150');
            }

            // Fetch books based on query
            function fetchBooks(query = 'biography') {
                if (isLoading) return;

                isLoading = true;
                const API_URL = `https://www.googleapis.com/books/v1/volumes?q=${query}&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

                // Introduce a delay to simulate a 0.2 second wait time
                setTimeout(() => {
                    fetch(API_URL)
                        .then(response => response.json())
                        .then(data => {
                            let books = data.items || [];
                            books = shuffleArray(books); // Randomize the order of books

                            const imagePromises = books.map(async book => {
                                const bookId = book.id;
                                if (!seenBooks.has(bookId)) {
                                    seenBooks.add(bookId);

                                    const bookInfo = book.volumeInfo;
                                    const bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : await fetchFallbackImage();

                                    const bookElement = document.createElement('div');
                                    bookElement.classList.add('book');

                                    bookElement.innerHTML = `
                                        <img src="${bookCover}" alt="${bookInfo.title}">
                                        <h3>${bookInfo.title}</h3>
                                        <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                        <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                                    `;

                                    booksContainer.appendChild(bookElement);
                                }
                            });

                            Promise.all(imagePromises).then(() => {
                                page++;
                                isLoading = false;
                            });
                        })
                        .catch(error => {
                            console.error('Error fetching the books:', error);
                            isLoading = false;
                        });
                }, 200); // 0.2 seconds delay
            }

            function handleScroll() {
                const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
                if (scrollTop + clientHeight >= scrollHeight - 5) {
                    fetchBooks(document.getElementById('searchInput').value || 'biography');
                }
            }

            function handleSearch() {
                // Reset page and clear books
                page = 1;
                seenBooks.clear();
                booksContainer.innerHTML = '';
                fetchBooks(document.getElementById('searchInput').value || 'biography');
            }

            // Load initial biography books
            fetchBooks();

            // Add event listener for infinite scrolling
            window.addEventListener('scroll', handleScroll);

            // Add event listener for search button
            document.getElementById('searchButton').addEventListener('click', handleSearch);

            // Add event listener for input changes
            document.getElementById('searchInput').addEventListener('input', () => {
                handleSearch();
            });
        });
    </script>
</body>
</html>

```
contact.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Contact Information</title>
  <style>
    body {
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    color: #ffffff;
    position: relative;
    overflow: hidden;
    background-image: url('front.png');
    backdrop-filter: blur(5px);
  }
  
  /* Background Image */
  body::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-size: cover;
    filter: brightness(0.5) blur(3px);
    z-index: -1;
  }
  
  /* Container Styles */
  .container {
    text-align: center;
    border: 4px solid;
    padding: 2rem;
    border-radius: 15px;
    width: 90%;
    max-width: 400px;
    background-image: url("https://img.freepik.com/free-vector/elegant-blurry-red-bokeh-lights-background_1017-25385.jpg?ga=GA1.1.309381817.1725084589&semt=ais_hybrid");
    background-size: cover;
    background-position: center;
    position: relative;
    box-shadow: 0 0 20px #9C0101;
    animation: neon-border 1.5s infinite alternate;
    z-index: 2;
    margin: 20px;
    box-sizing: border-box;
  }
  
  /* Header Styles */
  .header {
    color: #ADD8E6;
    margin-bottom: 1rem;
    font-size: 2rem;
    text-shadow: 2px 2px #555;
    line-height: 1.2;
  }
  
  /* Content Styles */
  .content {
    text-align: left;
    font-size: 1rem;
    line-height: 1.5;
    margin-bottom: 1rem;
  }
  
  /* Contact Item Styles */
  .contact-item {
    margin-bottom: 0.5rem;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
  }
  
  /* Icon Styles */
  .icon {
    font-size: 1.2rem;
    margin-right: 0.5rem;
  }
  
  /* Neon Border Animation */
  @keyframes neon-border {
    0% {
      border-color: #9C0101;
      box-shadow: 0 0 5px #9C0101, 0 0 10px #9C0101, 0 0 20px #9C0101, 0 0 30px #9C0101;
    }
    100% {
      border-color: #fff;
      box-shadow: 0 0 20px #ffffff, 0 0 30px #ffffff, 0 0 40px #ffffff, 0 0 50px #ffffff;
    }
  }
  
  /* Responsive Adjustments */
  @media screen and (max-width: 768px) {
    .container {
      width: 80%;
      padding: 1.5rem;
    }
  
    .header {
      font-size: 1.8rem;
    }
  
    .content {
      font-size: 0.9rem;
    }
  
    .icon {
      font-size: 1rem;
    }
  }
  
  @media screen and (max-width: 480px) {
    .container {
      width: 95%;
      padding: 1rem;
    }
  
    .header {
      font-size: 1.5rem;
    }
  
    .content {
      font-size: 0.8rem;
    }
  
    .icon {
      font-size: 0.9rem;
    }
  }
  </style>
</head>
<body>
  <!-- Main Content -->
  <div class="container">
    <h1 class="header">Contact Us</h1>
    <div class="content">
      <!-- Phone Numbers -->
      <div class="contact-item">
        <span class="icon">&#128222;</span> <!-- Phone Icon -->
        <p>Phone: 9875430927</p>
      </div>
      <div class="contact-item">
        <span class="icon">&#128222;</span> <!-- Phone Icon -->
        <p>Customer Care: +91 2228889997</p>
      </div>

      <!-- Email Addresses -->
      <div class="contact-item">
        <span class="icon">&#9993;</span> <!-- Email Icon -->
        <p>Email: khushiverma23@navgurukul.org</p>
      </div>
      <div class="contact-item">
        <span class="icon">&#9993;</span> <!-- Email Icon -->
        <p>Email: aditidewangan23@navgurukul.org</p>
      </div>
      <div class="contact-item">
        <span class="icon">&#9993;</span> <!-- Email Icon -->
        <p>Email: gopikashankh23@navgurukul.org</p>
      </div>
      <div class="contact-item">
        <span class="icon">&#9993;</span> <!-- Email Icon -->
        <p>Email: vaishalibarman23@navgurukul.org</p>
      </div>
    </div>
  </div>
</body>
</html>
```
Dystopian.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dystopian Books</title>
    <link rel="stylesheet" href="style.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: auto;
            overflow: hidden;
            padding-top: 100px;
        }

        #books {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .book {
            background: white;
            border: 1px solid #ddd;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .book img {
            max-width: 100%;
            height: auto;
        }

        .book h3 {
            font-size: 18px;
            margin: 10px 0;
        }

        .book p {
            font-size: 14px;
            color: #666;
        }

    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Reference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <div class="container">
            <h1>Dystopian Books</h1>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let page = 1;
            const booksContainer = document.getElementById('books');
            let isLoading = false;
            const seenBooks = new Set(); // To track seen book IDs

            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            function fetchBooks(query = '') {
                if (isLoading) return;

                isLoading = true;
                const API_URL = `https://www.googleapis.com/books/v1/volumes?q=${query}&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

                fetch(API_URL)
                    .then(response => response.json())
                    .then(data => {
                        let books = data.items || [];
                        books = shuffleArray(books); // Randomize the order of books

                        books.forEach(book => {
                            const bookId = book.id;
                            if (!seenBooks.has(bookId)) {
                                seenBooks.add(bookId);

                                const bookInfo = book.volumeInfo;
                                const bookElement = document.createElement('div');
                                bookElement.classList.add('book');

                                const bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : 'https://via.placeholder.com/150';
                                bookElement.innerHTML = `
                                    <img src="${bookCover}" alt="${bookInfo.title}">
                                    <h3>${bookInfo.title}</h3>
                                    <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                    <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                                `;

                                booksContainer.appendChild(bookElement);
                            }
                        });

                        page++;
                        isLoading = false;
                    })
                    .catch(error => {
                        console.error('Error fetching the books:', error);
                        isLoading = false;
                    });
            }

            function handleScroll() {
                const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
                if (scrollTop + clientHeight >= scrollHeight - 5) {
                    fetchBooks(document.getElementById('searchInput').value || 'dystopian');
                }
            }

            function handleSearch() {
                // Reset page and clear books
                page = 1;
                seenBooks.clear();
                booksContainer.innerHTML = '';
                fetchBooks(document.getElementById('searchInput').value || 'dystopian');
            }

            // Load initial books
            fetchBooks('dystopian');

            // Add event listener for infinite scrolling
            window.addEventListener('scroll', handleScroll);

            // Add event listener for search button
            document.getElementById('searchButton').addEventListener('click', handleSearch);

            // Add event listener for input changes
            document.getElementById('searchInput').addEventListener('input', () => {
                handleSearch();
            });
        });
    </script>
</body>
</html>
```
Fantasy.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fantasy Books</title>
    <link rel="stylesheet" href="style.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: auto;
            overflow: hidden;
            padding-top: 100px;
        }

        #books {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .book {
            background: white;
            border: 1px solid #ddd;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .book img {
            max-width: 100%;
            height: auto;
        }

        .book h3 {
            font-size: 18px;
            margin: 10px 0;
        }

        .book p {
            font-size: 14px;
            color: #666;
        }
    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Preference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <div class="container">
            <h1>Fantasy Books</h1>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let page = 1;
            const booksContainer = document.getElementById('books');
            let isLoading = false;
            const seenBooks = new Set(); // To track seen book IDs

            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            function fetchBooks(query = 'fantasy') {
                if (isLoading) return;

                isLoading = true;
                const API_URL = `https://www.googleapis.com/books/v1/volumes?q=${query}&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

                setTimeout(() => {
                    fetch(API_URL)
                        .then(response => response.json())
                        .then(data => {
                            let books = data.items || [];
                            books = shuffleArray(books); // Randomize the order of books

                            books.forEach(book => {
                                const bookId = book.id;
                                if (!seenBooks.has(bookId)) {
                                    seenBooks.add(bookId);

                                    const bookInfo = book.volumeInfo;
                                    const bookElement = document.createElement('div');
                                    bookElement.classList.add('book');

                                    const bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : 'https://via.placeholder.com/150';
                                    bookElement.innerHTML = `
                                        <img src="${bookCover}" alt="${bookInfo.title}">
                                        <h3>${bookInfo.title}</h3>
                                        <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                        <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                                    `;

                                    booksContainer.appendChild(bookElement);
                                }
                            });

                            page++;
                            isLoading = false;
                        })
                        .catch(error => {
                            console.error('Error fetching the books:', error);
                            isLoading = false;
                        });
                }, 200); // 0.2 seconds delay
            }

            function handleScroll() {
                const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
                if (scrollTop + clientHeight >= scrollHeight - 5) {
                    fetchBooks(document.getElementById('searchInput').value || 'fantasy');
                }
            }

            function handleSearch() {
                // Reset page and clear books
                page = 1;
                seenBooks.clear();
                booksContainer.innerHTML = '';
                fetchBooks(document.getElementById('searchInput').value || 'fantasy');
            }

            // Load initial books
            fetchBooks('fantasy');

            // Add event listener for infinite scrolling
            window.addEventListener('scroll', handleScroll);

            // Add event listener for search button
            document.getElementById('searchButton').addEventListener('click', handleSearch);

            // Add event listener for input changes
            document.getElementById('searchInput').addEventListener('input', () => {
                handleSearch();
            });
        });
    </script>
</body>
</html>
```
fiction.html
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fiction Books</title>
    <link rel="stylesheet" href="style.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: auto;
            overflow: hidden;
            padding-top: 100px;
        
        }
        .heading{
            display: grid;
            place-items: center;
            height: 50px;
        }
        h1{
            margin: 0;
            /* position: fixed; */
        }

        #books {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .book {
            background: white;
            border: 1px solid #ddd;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .book img {
            max-width: 100%;
            height: auto;
        }

        .book h3 {
            font-size: 18px;
            margin: 10px 0;
        }

        .book p {
            font-size: 14px;
            color: #666;
        }

        body {
            display: flex;
            flex-direction: column;
            font-family: Arial, sans-serif;
            background-color: #CEE6F2;
        }

        header {
            background-color: #962E2A;
            color: white;
            padding: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: fixed;
            width: 100%;
            height: 50px;
        }

        .header-left {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        header .logo {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            overflow: hidden;
        }

        header .logo img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .header-text {
            font-size: 24px;
            font-weight: bold;
            padding: 5px;
            border-radius: 5px;
        }

        header nav ul {
            list-style: none;
            display: flex;
            gap: 15px;
        }

        header nav ul li a {
            color: white;
            text-decoration: none;
            padding: 5px;
            border-radius: 5px;
        }

        header .search-bar {
            display: flex;
            gap: 5px;
            align-items: center;
        }

        header .search-bar select,
        header .search-bar input,
        header .search-bar button {
            border: 2px solid #962E2A;
            border-radius: 5px;
        }

        header .search-bar select,
        header .search-bar input {
            padding: 5px;
            font-size: 16px;
        }

        header .search-bar button {
            padding: 5px 10px;
            background-color: #555;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        header .search-bar button:hover {
            background-color: #777;
        }

        html,
        body {
            height: 100%;
            margin: 0;
        }

        body {
            display: flex;
            flex-direction: column;
        }

        main {
            flex: 1;
        }

        footer {
            background-color: #962E2A;
            color: white;
            text-align: center;
            padding: 10px;
            border-top: 2px solid #962E2A;
            position: fixed;
            bottom: 0;
            width: 100%;
        }


        footer {
            background-color: #962E2A;
            color: white;
            text-align: center;
            padding: 10px;
            border-top: 2px solid #962E2A;
        }

        .results-container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            justify-content: center;
            margin-top: 20px;

        }

        .result-item {
            border: 1px solid #ddd;
            border-radius: 10px;
            overflow: hidden;
            background: white;
            max-width: 300px;
            text-align: center;
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;

        }

        .result-item img {
            width: 128px;
            height: 192px;
            object-fit: cover;

        }

        .result-item .info {
            padding: 10px;
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;

        }

        .result-item .title {
            font-size: 18px;
            font-weight: bold;
        }

        .result-item .author {
            font-size: 16px;
            color: #555;
        }


        .result-item .description {
            font-size: 14px;
            color: #777;
            max-height: 6em;
            /* Adjust this value to fit 5 lines of text */
            overflow: hidden;
            /* Hide overflow text */
            text-overflow: ellipsis;
            /* Add ellipsis for overflow text */
            line-height: 1.2em;
        }
    </style>
</head>

<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Preference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <div class="container">
            <div class="heading">
                <h1>Fiction Books</h1>
            </div>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            let page = 1;
            const booksContainer = document.getElementById('books');
            let isLoading = false;
            const seenBooks = new Set(); // To track seen book IDs

            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            function fetchBooks(query = '') {
                if (isLoading) return;

                isLoading = true;
                const API_URL = `https://www.googleapis.com/books/v1/volumes?q=${query}+subject:fiction&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

                fetch(API_URL)
                    .then(response => response.json())
                    .then(data => {
                        let books = data.items || [];
                        books = shuffleArray(books); // Randomize the order of books

                        books.forEach(book => {
                            const bookId = book.id;
                            if (!seenBooks.has(bookId)) {
                                seenBooks.add(bookId);

                                const bookInfo = book.volumeInfo;
                                const bookElement = document.createElement('div');
                                bookElement.classList.add('book');

                                const bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : 'https://via.placeholder.com/150';
                                bookElement.innerHTML = `
                                    <img src="${bookCover}" alt="${bookInfo.title}">
                                    <h3>${bookInfo.title}</h3>
                                    <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                    <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                                `;

                                booksContainer.appendChild(bookElement);
                            }
                        });

                        page++;
                        isLoading = false;
                    })
                    .catch(error => {
                        console.error('Error fetching the books:', error);
                        isLoading = false;
                    });
            }

            function handleScroll() {
                const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
                if (scrollTop + clientHeight >= scrollHeight - 5) {
                    fetchBooks(document.getElementById('searchInput').value || 'fiction');
                }
            }

            function handleSearch() {
                // Reset page and clear books
                page = 1;
                seenBooks.clear();
                booksContainer.innerHTML = '';
                const query = document.getElementById('searchInput').value;
                if (query) {
                    fetchBooks(query);
                } else {
                    fetchBooks('fiction'); // Default to fiction books if input is empty
                }
            }

            // Load initial fiction books
            fetchBooks('fiction');

            // Add event listener for infinite scrolling
            window.addEventListener('scroll', handleScroll);

            // Add event listener for search button
            document.getElementById('searchButton').addEventListener('click', handleSearch);

            // Add event listener for input changes
            document.getElementById('searchInput').addEventListener('input', () => {
                handleSearch();
            });
        });
    </script>
</body>

</html>
```
index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login-Signup</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            font-family: sans-serif;
        }

        body::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-image: url("https://i.makeagif.com/media/3-26-2020/4A2aZE.gif");
            background-size: cover;
            filter: blur(3px);
            z-index: -1;
        }
        a{
            text-decoration: none;
        }

        #main {
            width: 600px;
            height: 700px;
            background-image: url("https://img.freepik.com/free-photo/red-wooden-background_53876-89000.jpg?ga=GA1.1.309381817.1725084589&semt=ais_hybrid");
            background-repeat: no-repeat;
            background-size: cover;
            overflow: hidden;
            border-radius: 10px;
            box-shadow: 10px 40px 80px rgba(0, 0, 0, 0.959);
        }

        #check {
            display: none;
        }

        .signup {
            position: relative;
            width: 100%;
            height: 100%;
        }

        label {
            color: #cbf3e5;
            font-size: 50px;
            justify-content: center;
            display: flex;
            margin-top: 50px;
            font-weight: bold;
            cursor: pointer;
            transition: color 0.5s ease-in-out;
            margin-bottom: 60px;
            animation: colorChange 3s infinite alternate ease-in-out;
        }

        @keyframes colorChange {
            0% {
                color: rgba(2, 61, 58, 0.959);
            }
            50% {
                color: #025753f5;
            }
            100% {
                color: #046a65f5;
            }
        }

        label:hover {
            color: #094737;
        }

        input {
            width: 60%;
            height: 20px;
            font-size: 20px;
            background: #e7bdd5;
            justify-content: center;
            display: flex;
            margin: 20px auto;
            padding: 10px;
            border: none;
            outline: none;
            border-radius: 70px;
        }

        input:hover {
            border: 3px solid #094737;
        }

        button {
            width: 40%;
            height: 50px;
            margin: 40px auto;
            justify-content: center;
            display: block;
            color: #cbf3e5;
            background: rgba(2, 61, 58, 0.959);
            font-size: 30px;
            font-weight: bold;
            margin-top: 40px;
            outline: none;
            border: none;
            border-radius: 40px;
            transition: .3s ease-in;
            cursor: pointer;
            box-shadow: 5px 20px 40px rgba(2, 85, 81, 0.959);
        }

        button:hover {
            background: #025551f5;
        }

        .login {
            height: 1000px;
            background: #d26868;
            border-radius: 60% / 10%;
            transform: translateY(-180px);
            transition: .8s ease-in-out;
        }

        .login h1 {
            color: #5e020c;
            transform: scale(.6);
        }

        .login p {
            text-align: center;
            padding: 30px;
        }

        .login input {
            background: rgb(94, 2, 12);
            color: white;
        }

        #check:checked ~ .login {
            transform: translateY(-500px);
        }

        #check:checked ~ .login label {
            transform: scale(1);
        }

        #check:checked ~ .signup label {
            transform: scale(.6);
        }
    </style>
</head>
<body>
    <div id="main">
        <input type="checkbox" id="check" aria-hidden="true">
        <div class="signup">
            <form id="signup-form" action="#">
                <label for="check" aria-hidden="true">Sign-Up</label>
                <div class="details">
                    <input type="text" name="username" placeholder="User-name" required>
                    <input type="email" name="email" id="email" placeholder="Email" required>
                    <input type="password" name="password" id="password" placeholder="Password" required>
                    <input type="password" name="confirm-password" id="confirm-password" placeholder="Confirm-password" required>
                </div>
                <button type="submit">Sign-Up</button>
            </form>
        </div>
        <div class="login">
            <form id="login-form" action="#">
                <label for="check" aria-hidden="true">Login</label>
                <div class="details">
                    <input type="email" name="email" id="login-email" placeholder="Email Or User name" required>
                    <input type="password" name="password" id="login-password" placeholder="Password" required>
                </div>
                <button type="submit">Login</button>
                <p>Don't have an account? <a href="index.html">Create account</a></p>
            </form>
        </div>
    </div>
    <script>
        // Sign-up form submission
        document.getElementById('signup-form').addEventListener('submit', function(e) {
            e.preventDefault();

            let username = e.target.username.value;
            let email = e.target.email.value.toLowerCase();
            let password = e.target.password.value;
            let confirmPassword = e.target['confirm-password'].value;

            let storedUsers = JSON.parse(localStorage.getItem('users')) || [];

            // Check if the user already exists
            let userExists = storedUsers.some(user => user.username === username || user.email === email);

            if (userExists) {
                alert('Username or Email already exists. Please use a different one.');
                return;
            }

            // Check if passwords match
            if (password !== confirmPassword) {
                alert('Passwords do not match!');
                return;
            }

            // Create a new user object
            let newUser = {
                username: username,
                email: email,
                password: password
            };

            // Store the new user in localStorage
            storedUsers.push(newUser);
            localStorage.setItem('users', JSON.stringify(storedUsers));
            alert('Sign up successful! You can now log in.');

            // Redirect to index1.html
            window.location.href = 'index1.html';

            // Reset the form
            e.target.reset();
        });

        // Login form submission
        document.getElementById('login-form').addEventListener('submit', function(e) {
            e.preventDefault();

            let loginEmail = e.target.email.value.toLowerCase();
            let loginPassword = e.target.password.value;

            let storedUsers = JSON.parse(localStorage.getItem('users')) || [];

            // Check if the user credentials are valid
            let validUser = storedUsers.find(user => 
                (user.email === loginEmail || user.username === loginEmail) && user.password === loginPassword
            );

            if (validUser) {
                alert('Login successful!');
                window.location.href = 'index1.html';
            } else {
                alert('Invalid credentials!');
            }
        });
    </script>
</body>
</html>
```
index1.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Library Management System</title>
    <style>
        
body {
    display: flex;
    flex-direction: column;
    font-family: Arial, sans-serif;
    background-color: #CEE6F2;
    min-height: 100vh;
    padding: 0%;
    margin: 0%;
}

/* Header Styling */
header {
    background-color: #962E2A;
    color: white;
    padding: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.header-left {
    display: flex;
    align-items: center;
    gap: 15px;
}

header .logo {
    width: 70px;
    height: 70px;
    border-radius: 50%;
    overflow: hidden;
}

header .logo img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.header-text {
    font-size: 24px;
    font-weight: bold;
    padding: 5px;
    border-radius: 5px;
}

header nav ul {
    list-style: none;
    display: flex;
    gap: 15px;
}

header nav ul li a {
    color: white;
    text-decoration: none;
    padding: 5px;
    border-radius: 5px;
}

header .search-bar {
    display: flex;
    gap: 5px;
    align-items: center;
}

header .search-bar input {
    border: 2px solid #962E2A;
    border-radius: 5px;
}
header .search-bar input {
    padding: 5px;
    font-size: 16px;
}

header .search-bar button {
    padding: 5px 10px;
    background-color: #555;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

header .search-bar button:hover {
    background-color: #777;
}

@media (max-width: 768px) {

    /* Stack header elements vertically on smaller screens */
    header {
        flex-direction: column;
        align-items: flex-start;
    }

    .header-left {
        margin-bottom: 10px;
        gap: 10px;
    }

    header nav ul {
        flex-direction: column;
        width: 100%;
        align-items: flex-start;
    }

    header nav ul li {
        margin-bottom: 10px;
    }

    .search-bar {
        width: 100%;
        flex-direction: column;
        gap: 10px;
    }

    .search-bar select,
    .search-bar input,
    .search-bar button {
        width: 100%;
    }


    @media (max-width: 480px) {
        .header-text {
            font-size: 20px;
        }

        header .logo {
            width: 50px;
            height: 50px;
        }
        header .search-bar input{
            font-size: 14px;
            padding: 8px;
        }
    }
}

/* Main Content Area Styling */
main {
    flex: 1;
    padding: 20px;
}header .search-bar button:hover {
    background-color: #777;
}

/* Genre List Styling */
#genre-list {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    margin-bottom: 30px;
    padding: 0 50px;
}

.genre-item {
    background-color: #f5f5f5;
    position: relative;
    height: 300px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    overflow: hidden;
    transition: transform 0.3s ease-in-out;
    border: 2px solid #962E2A;
    animation: none;
}

.genre-item img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1;
    background-color: #f5f5f5;
}

.genre-item span {
    position: relative;
    z-index: 2;
    display: block;
    text-align: center;
    padding: 10px;
    font-size: 18px;
    font-weight: bold;
    color: #333;
    background-color: rgba(255, 255, 255, 0.7);
}

.genre-item:hover {
    transform: scale(1.05);
    animation: neon-border 1.5s infinite alternate;
}

/* For tablets and smaller screens (max-width: 1024px) */
@media (max-width: 1024px) {
    #genre-list {
        grid-template-columns: repeat(3, 1fr);
        /* 3 columns */
        padding: 0 20px;
        /* Adjust padding */
    }

    .genre-item {
        height: 250px;
        /* Adjust height */
    }

    .genre-item span {
        font-size: 16px;
        /* Adjust font size */
        padding: 8px;
        /* Adjust padding */
    }
}

/* For mobile devices (max-width: 768px) */
@media (max-width: 768px) {
    #genre-list {
        grid-template-columns: repeat(2, 1fr);
        /* 2 columns */
        padding: 0 10px;
        /* Adjust padding */
    }

    .genre-item {
        height: 200px;
        /* Adjust height */
    }

    .genre-item span {
        font-size: 14px;
        /* Adjust font size */
        padding: 6px;
        /* Adjust padding */
    }
}

/* For very small mobile devices (max-width: 480px) */
@media (max-width: 480px) {
    #genre-list {
        grid-template-columns: 1fr;
        /* 1 column */
        padding: 0 5px;
        /* Adjust padding */
    }

    .genre-item {
        height: 150px;
        /* Adjust height */
    }

    .genre-item span {
        font-size: 12px;
        /* Adjust font size */
        padding: 4px;
        /* Adjust padding */
    }
}

/* Results Container Styling */
.results-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    grid-auto-rows: minmax(200px, auto);
    gap: 20px;
    justify-content: center;
    margin-top: 20px;
}
a{
    text-decoration: none;
}

.result-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    background: white;
    border: 2px solid #962e2a0e;
    border-radius: 10px;
    overflow: hidden;
    text-align: center;
    max-width: 100%;
    padding: 10px;
}

.result-item img {
    max-width: 100%;
    max-height: 200px;
    object-fit: cover;
    margin-bottom: 10px;
}

.result-item .info {
    padding: 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}

.result-item .title {
    font-size: 18px;
    font-weight: bold;
}

.result-item .author {
    font-size: 16px;
    color: #555;
}

.result-item .description {
    font-size: 14px;
    color: #777;
    max-height: 6em;
    overflow: hidden;
    text-overflow: ellipsis;
    line-height: 1.2em;
}

/* Neon Border Animation */
@keyframes neon-border {
    0% {
        border-color: #962E2A;
        box-shadow: 0 0 5px #962E2A;
    }

    100% {
        border-color: #D32F2F;
        box-shadow: 0 0 20px #D32F2F, 0 0 30px #D32F2F;
    }
}


footer {
    background-color: #962E2A;
    color: white;
    text-align: center;
    padding: 10px;
    border-top: 2px solid #962E2A;
    margin-top: auto;
    /* Pushes the footer to the bottom */
}

/* Responsive Design */
@media (max-width: 768px) {
    .results-container {
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    }

    footer {
        padding: 15px;
        /* Increased padding for better touch targets on smaller screens */
        font-size: 14px;
        /* Slightly smaller text size for better readability on mobile devices */
    }
}

@media (max-width: 480px) {
    footer {
        padding: 20px;
        /* More padding for better appearance on very small screens */
        font-size: 12px;
        /* Further reduced text size for very small screens */
    }
    header nav ul{
        display: flex;
        flex-direction: row !important;
        gap: 15px;
    }
}

    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Reference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <section id="genre-list">
            <a href="fiction.html"><div class="genre-item">
                <img src="https://png.pngtree.com/png-vector/20230728/ourmid/pngtree-novel-clipart-open-book-with-houses-cartoon-vector-png-image_6816048.png" alt="Genre">
                <span>Fiction</span>
            </div>
            </a>
            <a href="non-fiction.html"><div class="genre-item">
                <img src="https://images.twinkl.co.uk/tw1n/image/private/t_630/u/ux/fiction-wiki_ver_1.png" alt="Genre">
                <span>Non-Fiction</span>
            </div>
            </a>
            <a href="Dystopian.html"> <div class="genre-item">
                <img src="https://img.freepik.com/premium-photo/surrealistic-dystopian-concept-art-young-boy-with-books_899449-194011.jpg" alt="Genre">
                <span>Dystopian</span>
            </div>
            </a>
            <a href="Science_Fiction.html"><div class="genre-item">
                <img src="https://media.istockphoto.com/id/1753620559/vector/two-students-do-research-in-the-laboratory-vector-illustration.jpg?s=612x612&w=0&k=20&c=2xzDcgCERwOTurK-_7wtEeKTDUxZ7hEOx1xDqVsrlko=" alt="Genre">
                <span>Science Fiction</span>
            </div>
            </a>
            <a href="Mystery.html"><div class="genre-item">
                <img src="https://media.istockphoto.com/id/160178497/vector/detective-kids.jpg?s=612x612&w=0&k=20&c=Dg30gI1gm6e2Nr_VJ5ZK2IrOp4un6cu-3tDVHjeYkD4=" alt="Genre">
                <span>Mystery</span>
            </div>
            </a>
            <a href="Biography.html"><div class="genre-item">
                <img src="https://clipart-library.com/8300/1931/book-magic-spells-witchcraft_105738-762.jpg" alt="Genre">
                <span>Biography</span>
            </div>
            </a>
            <a href="Fantasy.html"><div class="genre-item">
                <img src="https://png.pngtree.com/png-vector/20231210/ourlarge/pngtree-christmas-day-fantasy-book-white-background-png-image_11197899.png" alt="Genre">
                <span>Fantasy</span>
            </div>
            </a>
            <a href="Romance.html"><div class="genre-item">
                <img src="https://img.freepik.com/premium-vector/romance-library-kiss-couple-while-reading-book_686079-14.jpg" alt="Genre">
                <span>Romance</span>
            </div>
            </a>
            
        </section>
        <div id="results" class="results-container"></div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>

    <script>

        
let timeoutId = null;
let startIndex = 0; // Start index for pagination
const maxResults = 20; // Number of results per API request

// Search input event listener with debounce
document.getElementById('searchInput').addEventListener('input', function () {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
        startIndex = 0; // Reset startIndex for a new search
        performSearch();
    }, 300); // 300ms debounce time
});

// Function to perform the search
function performSearch() {
    let query = document.getElementById('searchInput').value.trim();
    let genreItems = document.querySelectorAll('.genre-item');
    let resultsContainer = document.getElementById('results');

    // Check if resultsContainer exists
    if (!resultsContainer) {
        console.error('Results container not found');
        return;
    }

    console.log('Performing search with query:', query);

    if (!query) {
        resultsContainer.innerHTML = ''; // Clear search results
        genreItems.forEach(item => item.style.display = 'block'); // Show genre items
        return;
    }

    if (startIndex === 0) {
        resultsContainer.innerHTML = ''; // Clear previous results for a new search
    }

    // Check if genreItems exist
    if (genreItems.length > 0) {
        genreItems.forEach(item => item.style.display = 'none'); // Hide genre items when searching
    }

    // Construct the correct API URL with query, startIndex, and maxResults
    let apiUrl = `https://www.googleapis.com/books/v1/volumes?q=${encodeURIComponent(query)}&startIndex=${startIndex}&maxResults=${maxResults}`;

    console.log('API URL:', apiUrl);

    fetch(apiUrl)
        .then(response => {
            if (!response.ok) {
                throw new Error('Network response was not ok ' + response.statusText);
            }
            return response.json();
        })
        .then(data => {
            if (data.items && Array.isArray(data.items)) {
                // Filter for unique books
                const seenIds = new Set();
                const uniqueBooks = data.items.filter(book => {
                    if (seenIds.has(book.id)) {
                        return false; // Duplicate book, don't include it
                    }
                    seenIds.add(book.id); // Add the book's ID to the Set
                    return true; // Unique book, include it
                });

                // Display the unique books
                displayBooks(uniqueBooks);

                // Show or hide the "Load More" button based on the number of results
                if (data.items.length === maxResults) {
                    showLoadMoreButton();
                } else {
                    hideLoadMoreButton(); // Hide the button if no more results
                }
            } else {
                resultsContainer.innerHTML = 'No results found';
                hideLoadMoreButton();
            }
        })
        .catch(error => {
            console.error('Error fetching search results:', error.message);
            resultsContainer.innerHTML = 'Error fetching results: ' + error.message;
            hideLoadMoreButton();
        });
}

// Function to display books in the results container
function displayBooks(books) {
    const resultsContainer = document.getElementById('results');
    resultsContainer.innerHTML = ''; // Clear previous results

    books.forEach(book => {
        const item = document.createElement('div');
        item.className = 'result-item';

        const img = document.createElement('img');
        img.src = book.volumeInfo.imageLinks ? book.volumeInfo.imageLinks.thumbnail : 'https://via.placeholder.com/128x192?text=No+Image';
        img.alt = book.volumeInfo.title;

        const info = document.createElement('div');
        info.className = 'info';

        const title = document.createElement('div');
        title.className = 'title';
        title.textContent = book.volumeInfo.title;

        const author = document.createElement('div');
        author.className = 'author';
        author.textContent = book.volumeInfo.authors ? book.volumeInfo.authors.join(', ') : 'Unknown Author';

        // New: Adding description
        const description = document.createElement('div');
        description.className = 'description';
        description.textContent = book.volumeInfo.description ? book.volumeInfo.description : 'No description available';

        // Append title, author, and description to the info div
        info.appendChild(title);
        info.appendChild(author);
        info.appendChild(description);

        // Append image and info to the result item
        item.appendChild(img);
        item.appendChild(info);

        // Append the result item to the results container
        resultsContainer.appendChild(item);
    });
} 

// Function to show the "Load More" button
function showLoadMoreButton() {
    let loadMoreBtn = document.getElementById('loadMore');
    if (loadMoreBtn) {
        loadMoreBtn.style.display = 'block';
    }
}

// Function to hide the "Load More" button
function hideLoadMoreButton() {
    let loadMoreBtn = document.getElementById('loadMore');
    if (loadMoreBtn) {
        loadMoreBtn.style.display = 'none';
    }
}

// Event listener for "Load More" button click
document.getElementById('loadMore').addEventListener('click', function () {
    startIndex += maxResults; // Increment the startIndex to fetch the next set of results
    performSearch(); // Fetch more results
});
    </script>
</body>
</html>
```
Mystery.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mystery Books</title>
    <link rel = "stylesheet" href="style.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: auto;
            overflow: hidden;
        }

        #books {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .book {
            background: white;
            border: 1px solid #ddd;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .book img {
            max-width: 100%;
            height: auto;
        }

        .book h3 {
            font-size: 18px;
            margin: 10px 0;
        }

        .book p {
            font-size: 14px;
            color: #666;
        }

        .search-bar {
            text-align: center;
            margin: 20px 0;
        }

        .search-bar input {
            padding: 10px;
            font-size: 16px;
            width: 300px;
            margin-right: 10px;
        }

        .search-bar button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }

    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Reference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <div class="container">
            <h1>Mystery Books</h1>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let page = 1;
            const booksContainer = document.getElementById('books');
            let isLoading = false;
            const seenBooks = new Set(); // To track seen book IDs

            // Shuffle the books array to randomize positions
            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            function getFallbackImage(isbn) {
                return isbn ? `https://covers.openlibrary.org/b/isbn/${isbn}-L.jpg` : 'https://via.placeholder.com/150';
            }

            function fetchBooks(query = 'mystery') {
                if (isLoading) return;

                isLoading = true;
                const API_URL = `https://www.googleapis.com/books/v1/volumes?q=${query}&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

                setTimeout(() => {
                    fetch(API_URL)
                        .then(response => response.json())
                        .then(data => {
                            let books = data.items || [];
                            books = shuffleArray(books); // Randomize the order of books

                            books.forEach(book => {
                                const bookId = book.id;
                                if (!seenBooks.has(bookId)) {
                                    seenBooks.add(bookId);

                                    const bookInfo = book.volumeInfo;
                                    const bookElement = document.createElement('div');
                                    bookElement.classList.add('book');

                                    let bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : '';

                                    // Fallback to Open Library Covers API if Google Books image is not available
                                    if (!bookCover) {
                                        const isbn = bookInfo.industryIdentifiers ? bookInfo.industryIdentifiers.find(id => id.type === 'ISBN_13' || id.type === 'ISBN_10')?.identifier : null;
                                        bookCover = getFallbackImage(isbn);
                                    }

                                    bookElement.innerHTML = `
                                        <img src="${bookCover}" alt="${bookInfo.title}">
                                        <h3>${bookInfo.title}</h3>
                                        <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                        <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                                    `;

                                    booksContainer.appendChild(bookElement);
                                }
                            });

                            page++;
                            isLoading = false;
                        })
                        .catch(error => {
                            console.error('Error fetching the books:', error);
                            isLoading = false;
                        });
                }, 200);
            }

            function handleScroll() {
                const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
                if (scrollTop + clientHeight >= scrollHeight - 5) {
                    fetchBooks(document.getElementById('searchInput').value || 'mystery');
                }
            }

            function handleSearch() {
                // Reset page and clear books
                page = 1;
                seenBooks.clear();
                booksContainer.innerHTML = '';
                fetchBooks(document.getElementById('searchInput').value || 'mystery');
            }

            // Load initial mystery books
            fetchBooks();

            // Add event listener for infinite scrolling
            window.addEventListener('scroll', handleScroll);

            // Add event listener for search button
            document.getElementById('searchButton').addEventListener('click', handleSearch);

            // Add event listener for input changes
            document.getElementById('searchInput').addEventListener('input', () => {
                handleSearch();
            });
        });
    </script>
</body>
</html>
```
non-fiction.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Non-Fiction Books</title>
    <link rel="stylesheet" href="style.css">
    <style>
        body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 0;
}

.heading {
    display: grid;
    place-items: center;    
    height: 50px; 
}

h1 {
    margin: 0; 
    position: fixed;
}

.container {
    width: 80%;
    margin: auto;
    overflow: hidden;
    padding-top: 100px;
}

#books {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    margin-bottom: 20px;
}

.book {
    background: white;
    border: 1px solid #ddd;
    margin: 10px;
    padding: 15px;
    width: 200px;
    text-align: center;
}

.book img {
    max-width: 100%;
    height: auto;
}

.book h3 {
    font-size: 18px;
    margin: 10px 0;
}

.book p {
    font-size: 14px;
    color: #666;
}

    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Reference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <div class="container">
            <div class="heading">
                <h1>Non-Fiction Books</h1>
            </div>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
    document.addEventListener('DOMContentLoaded', function() {
    let page = 1;
    const booksContainer = document.getElementById('books');
    let isLoading = false;
    const seenBooks = new Set(); // To track seen book IDs
    const loadingIndicator = document.createElement('div');
    loadingIndicator.classList.add('loading');
    loadingIndicator.textContent = 'Loading...';

    function fetchBooks(query = '') {
        if (isLoading) return;

        isLoading = true;
        booksContainer.appendChild(loadingIndicator);

        const API_URL = `https://www.googleapis.com/books/v1/volumes?q=subject:nonfiction+${query}&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

        const fetchTimeout = setTimeout(() => {
            fetch(API_URL)
                .then(response => response.json())
                .then(data => {
                    const books = data.items || [];

                    books.forEach(book => {
                        const bookId = book.id;
                        if (!seenBooks.has(bookId)) {
                            seenBooks.add(bookId);

                            const bookInfo = book.volumeInfo;
                            const bookElement = document.createElement('div');
                            bookElement.classList.add('book');

                            const bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : 'https://via.placeholder.com/150';
                            bookElement.innerHTML = `
                                <img src="${bookCover}" alt="${bookInfo.title}">
                                <h3>${bookInfo.title}</h3>
                                <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                            `;

                            booksContainer.appendChild(bookElement);
                        }
                    });

                    page++;
                    isLoading = false;
                    booksContainer.removeChild(loadingIndicator);
                })
                .catch(error => {
                    console.error('Error fetching the books:', error);
                    isLoading = false;
                    booksContainer.removeChild(loadingIndicator);
                });
        }, 100);
    }

    function handleScroll() {
        const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
        if (scrollTop + clientHeight >= scrollHeight - 5) {
            fetchBooks(document.getElementById('searchInput').value);
        }
    }

    function handleSearch() {
        // Reset page and clear books
        page = 1;
        seenBooks.clear();
        booksContainer.innerHTML = '';
        fetchBooks(document.getElementById('searchInput').value);
    }

    // Load initial books
    fetchBooks();

    // Add event listener for infinite scrolling
    window.addEventListener('scroll', handleScroll);

    // Add event listener for search button
    document.getElementById('searchButton').addEventListener('click', handleSearch);

    // Add event listener for input changes
    document.getElementById('searchInput').addEventListener('input', () => {
        handleSearch();
    });
});


    </script>
</body>
</html>
```
Romance.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Romance Books</title>
    <link rel="stylesheet" href="style.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: auto;
            overflow: hidden;
            padding-top: 100px;
        }

        #books {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .book {
            background: white;
            border: 1px solid #ddd;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .book img {
            max-width: 100%;
            height: auto;
        }

        .book h3 {
            font-size: 18px;
            margin: 10px 0;
        }

        .book p {
            font-size: 14px;
            color: #666;
        }
    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="index1.html">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Reference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            
        </div>
    </header>
    <main>
        <div class="container">
            <h1>Romance Books</h1>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let page = 1;
            const booksContainer = document.getElementById('books');
            let isLoading = false;
            const seenBooks = new Set(); // To track seen book IDs

            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            function fetchBooks(query = 'romance') {
                if (isLoading) return;

                isLoading = true;
                const API_URL = `https://www.googleapis.com/books/v1/volumes?q=${query}&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

                setTimeout(() => {
                    fetch(API_URL)
                        .then(response => response.json())
                        .then(data => {
                            let books = data.items || [];
                            books = shuffleArray(books); // Randomize the order of books

                            books.forEach(book => {
                                const bookId = book.id;
                                if (!seenBooks.has(bookId)) {
                                    seenBooks.add(bookId);

                                    const bookInfo = book.volumeInfo;
                                    const bookElement = document.createElement('div');
                                    bookElement.classList.add('book');

                                    const bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : 'https://via.placeholder.com/150';
                                    bookElement.innerHTML = `
                                        <img src="${bookCover}" alt="${bookInfo.title}">
                                        <h3>${bookInfo.title}</h3>
                                        <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                        <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                                    `;

                                    booksContainer.appendChild(bookElement);
                                }
                            });

                            page++;
                            isLoading = false;
                        })
                        .catch(error => {
                            console.error('Error fetching the books:', error);
                            isLoading = false;
                        });
                }, 200); // 0.2 seconds delay
            }

            function handleScroll() {
                const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
                if (scrollTop + clientHeight >= scrollHeight - 5) {
                    fetchBooks(document.getElementById('searchInput').value || 'romance');
                }
            }

            function handleSearch() {
                // Reset page and clear books
                page = 1;
                seenBooks.clear();
                booksContainer.innerHTML = '';
                fetchBooks(document.getElementById('searchInput').value || 'romance');
            }

            // Load initial books
            fetchBooks('romance');

            // Add event listener for infinite scrolling
            window.addEventListener('scroll', handleScroll);

            // Add event listener for search button
            document.getElementById('searchButton').addEventListener('click', handleSearch);

            // Add event listener for input changes
            document.getElementById('searchInput').addEventListener('input', () => {
                handleSearch();
            });
        });
    </script>
</body>
</html>
```
Science_Fiction.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Science Fiction Books</title>
    <link rel="stylesheet" href="style.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: auto;
            overflow: hidden;
            padding-top: 100px;
        }

        #books {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .book {
            background: white;
            border: 1px solid #ddd;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .book img {
            max-width: 100%;
            height: auto;
        }

        .book h3 {
            font-size: 18px;
            margin: 10px 0;
        }

        .book p {
            font-size: 14px;
            color: #666;
        }

    </style>
</head>
<body>
    <header>
        <div class="header-left">
            <div class="logo">
                <img src="logo.png" alt="Library Logo">
            </div>
            <div class="header-text">BookBuddy</div>
        </div>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="https://www.pdfdrive.com/">Reference</a></li>
                <li><a href="contact.html">Contact</a></li>
            </ul>
        </nav>
        <div class="search-bar">
            
            <input type="text" id="searchInput" placeholder="Search...">
            <button id="searchButton">Search</button>
        </div>
    </header>
    <main>
        <div class="container">
            <h1>Science Fiction Books</h1>
            <div id="books"></div>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Library Management System. All rights reserved.</p>
    </footer>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let page = 1;
            const booksContainer = document.getElementById('books');
            let isLoading = false;
            const seenBooks = new Set(); // To track seen book IDs

            // Shuffle the books array to randomize positions
            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            // Fetch books based on query
            function fetchBooks(query = 'science+fiction') {
                if (isLoading) return;

                isLoading = true;
                const API_URL = `https://www.googleapis.com/books/v1/volumes?q=${query}&orderBy=relevance&startIndex=${(page - 1) * 10}&maxResults=10`;

                // Introduce a delay to simulate a 0.2 second wait time
                setTimeout(() => {
                    fetch(API_URL)
                        .then(response => response.json())
                        .then(data => {
                            let books = data.items || [];
                            books = shuffleArray(books); // Randomize the order of books

                            books.forEach(book => {
                                const bookId = book.id;
                                if (!seenBooks.has(bookId)) {
                                    seenBooks.add(bookId);

                                    const bookInfo = book.volumeInfo;
                                    const bookElement = document.createElement('div');
                                    bookElement.classList.add('book');

                                    const bookCover = bookInfo.imageLinks ? bookInfo.imageLinks.thumbnail : 'https://via.placeholder.com/150';
                                    bookElement.innerHTML = `
                                        <img src="${bookCover}" alt="${bookInfo.title}">
                                        <h3>${bookInfo.title}</h3>
                                        <p>by ${bookInfo.authors ? bookInfo.authors.join(', ') : 'Unknown'}</p>
                                        <p>Rating: ${bookInfo.averageRating || 'N/A'}</p>
                                    `;

                                    booksContainer.appendChild(bookElement);
                                }
                            });

                            page++;
                            isLoading = false;
                        })
                        .catch(error => {
                            console.error('Error fetching the books:', error);
                            isLoading = false;
                        });
                }, 200); // 0.2 seconds delay
            }

            function handleScroll() {
                const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
                if (scrollTop + clientHeight >= scrollHeight - 5) {
                    fetchBooks(document.getElementById('searchInput').value || 'science+fiction');
                }
            }

            function handleSearch() {
                // Reset page and clear books
                page = 1;
                seenBooks.clear();
                booksContainer.innerHTML = '';
                fetchBooks(document.getElementById('searchInput').value || 'science+fiction');
            }

            // Load initial science fiction books
            fetchBooks();

            // Add event listener for infinite scrolling
            window.addEventListener('scroll', handleScroll);

            // Add event listener for search button
            document.getElementById('searchButton').addEventListener('click', handleSearch);

            // Add event listener for input changes
            document.getElementById('searchInput').addEventListener('input', () => {
                handleSearch();
            });
        });
    </script>
</body>
</html>
```
style.css
```css
body {
    display: flex;
    flex-direction: column;
    font-family: Arial, sans-serif;
    background-color: #CEE6F2;
    margin: 0;
    padding: 0;
}

header {
    background-color: #962E2A;
    color: white;
    padding: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: fixed;
    width: 100%;
    height: 50px;
    z-index: 1000;
}

.header-left {
    display: flex;
    align-items: center;
    gap: 15px;
}

header .logo {
    width: 70px;
    height: 70px;
    border-radius: 50%;
    overflow: hidden;
}

header .logo img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.header-text {
    font-size: 24px;
    font-weight: bold;
    padding: 5px;
    border-radius: 5px;
}

header nav ul {
    list-style: none;
    display: flex;
    gap: 15px;
    margin: 0;
    padding: 0;
}

header nav ul li a {
    color: white;
    text-decoration: none;
    padding: 5px;
    border-radius: 5px;
}

.search-bar{
    margin-right: 50px;
}

header .search-bar {
    display: flex;
    gap: 5px;
    align-items: center;
}

header .search-bar select,
header .search-bar input,
header .search-bar button {
    border: 2px solid #962E2A;
    border-radius: 5px;
}

header .search-bar select,
header .search-bar input {
    padding: 5px;
    font-size: 16px;
}

header .search-bar button {
    padding: 5px 10px;
    background-color: #555;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

header .search-bar button:hover {
    background-color: #777;
}

html, body {
    height: 100%;
    margin: 0;
}

body {
    display: flex;
    flex-direction: column;
}

main {
    flex: 1;
    margin-top: 70px; /* Adjust to ensure content isn't hidden behind header */
}

footer {
    background-color: #962E2A;
    color: white;
    text-align: center;
    padding: 10px;
    border-top: 2px solid #962E2A;
    position: fixed;
    bottom: 0;
    width: 100%;
}

.results-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
    margin-top: 20px;
    padding-bottom: 60px; /* Ensure content isn't hidden behind footer */
}

.result-item {
    border: 1px solid #ddd;
    border-radius: 10px;
    overflow: hidden;
    background: white;
    max-width: 300px;
    text-align: center;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.result-item img {
    width: 128px;
    height: 192px;
    object-fit: cover;
    margin: auto;
}

.result-item .info {
    padding: 10px;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.result-item .title {
    font-size: 18px;
    font-weight: bold;
}

.result-item .author {
    font-size: 16px;
    color: #555;
}

.result-item .description {
    font-size: 14px;
    color: #777;
    max-height: 6em; /* Adjust this value to fit 5 lines of text */
    overflow: hidden; /* Hide overflow text */
    text-overflow: ellipsis; /* Add ellipsis for overflow text */
    line-height: 1.2em;
}

@media (width < 600px) {
    header {
        flex-direction: column;
        height: auto;
    }

    .header-left {
        justify-content: center;
        margin-bottom: 10px;
    }

    header nav ul {
        flex-direction: row;
        gap: 10px;
        margin-top: 5px;
    }

    .search-bar {
        flex-direction: column;
        width: 80%;
        margin-right: 20px;
        margin-top: 5px;
    }

    header .search-bar select,
    header .search-bar input,
    header .search-bar button {
        width: 100%;
    }

    .results-container {
        flex-direction: column;
    }

    .result-item {
        max-width: 100%;
        margin: 0 auto;
    }

    footer {
        padding: 5px;
    }
}

/* Tablets and small desktops */
@media (610px <= width <= 1024px) {
    .results-container {
        gap: 15px;
    }

    .result-item {
        max-width: 45%; /* Adjust to fit 2 items per row */
    }

    .result-item img {
        width: 100px;
        height: 150px;
    }
}

/* Larger desktops */
@media (width >= 1025px) {
    .results-container {
        gap: 20px;
    }

    .result-item {
        max-width: 30%; /* Adjust to fit 3 items per row */
    }

    .result-item img {
        width: 128px;
        height: 192px;
    }
}
```