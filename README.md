Khristin Schenk<br>
Feb 10th, 2025<br>
SDEV-220<br>

## Module 4 Lab - Case Study: *Python APIs*

### Instructions
*After watching the video by Caleb Curry, create a CRUD API for a Book instead of a Drink, as demonstrated in the video example.*

**The Book model should have the following parameters:**
- id
- book_name
- author
- publisher

> YouTube Video by Caleb Curry: *[REST API Crash Course - Introduction + Full Python API Tutorial](https://www.youtube.com/watch?v=qbLc5a9jdXo&ab_channel=CalebCurry)*


## NOTES

#### Pseudocode (V.01)

```pseudocode
Initialize Flask app
Configure app to use SQLite database
Initialize SQLAlchemy with app

Define Book class with id, book_name, author, and publisher attributes

Create database tables with Book schema

Define route to create a new book
    Parse JSON data from request
    Create new Book object with data
    Add new book to session
    Commit changes to database
    Return JSON response with new book id

Define route to get all books
    Query all books from database
    Return JSON response with list of books

Define route to get a specific book by id
    Query book with specified id from database
    Return JSON response with book details

Define route to update a book by id
    Query book with specified id from database
    Parse JSON data from request
    Update book attributes with new data
    Commit changes to database
    Return JSON response with success message

Define route to delete a book by id
    Query book with specified id from database
    Delete book from session
    Commit changes to database
    Return JSON response with success message

Run app in debug mode
```

---

#### Python Code (V.01)

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///books.db'
db = SQLAlchemy(app)

class Book(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    book_name = db.Column(db.String(100))
    author = db.Column(db.String(100))
    publisher = db.Column(db.String(100))

with app.app_context():
    db.create_all()

@app.route('/books', methods=['POST'])
def create_book():
    data = request.get_json()
    new_book = Book(book_name=data['book_name'], author=data['author'], publisher=data['publisher'])
    db.session.add(new_book)
    db.session.commit()
    return jsonify({"id": new_book.id}), 201

@app.route('/books', methods=['GET'])
def get_books():
    books = Book.query.all()
    return jsonify([{"id": book.id, "book_name": book.book_name, "author": book.author, "publisher": book.publisher} for book in books])

@app.route('/books/<int:id>', methods=['GET'])
def get_book(id):
    book = Book.query.get(id)
    return jsonify({"id": book.id, "book_name": book.book_name, "author": book.author, "publisher": book.publisher})

@app.route('/books/<int:id>', methods=['PUT'])
def update_book(id):
    book = Book.query.get(id)
    data = request.get_json()
    book.book_name = data.get('book_name', book.book_name)
    book.author = data.get('author', book.author)
    book.publisher = data.get('publisher', book.publisher)
    db.session.commit()
    return jsonify({"message": "Book updated"})

@app.route('/books/<int:id>', methods=['DELETE'])
def delete_book(id):
    book = Book.query.get(id)
    db.session.delete(book)
    db.session.commit()
    return jsonify({"message": "Book deleted"})

if __name__ == '__main__':
    app.run(debug=True)
```


---

##  References

Omareeo. (2025). _Flask CRUD API for data management._ GitHub. Retrieved from [https://github.com/Omareeo/Python-Flask-CRUD-API](https://github.com/Omareeo/Python-Flask-CRUD-API)

SQLAlchemy authors & contributors. (2025). _SQLAlchemy overview._ SQLAlchemy Documentation. Retrieved from [https://docs.sqlalchemy.org/en/20/intro.html](https://docs.sqlalchemy.org/en/20/intro.html)

SQLAlchemy authors & contributors. (2025). _SQLAlchemy installation guide._ SQLAlchemy Documentation. Retrieved from [https://docs.sqlalchemy.org/en/20/intro.html#installation](https://docs.sqlalchemy.org/en/20/intro.html#installation)

Curry, C. (2025, Month Day). _REST API crash course - Introduction + full Python API tutorial_ [Video]. YouTube. Retrieved from [https://www.youtube.com/watch?v=qbLc5a9jdXo&ab_channel=CalebCurry](https://www.youtube.com/watch?v=qbLc5a9jdXo&ab_channel=CalebCurry)



---

> Written with [StackEdit](https://stackedit.io/).
