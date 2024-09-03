# E-COMMERCE-
from flask import Flask, render_template, request, redirect, url_for
import sqlite3

app = Flask(_name_)

# Database setup function
def init_db():
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS products (
                        id INTEGER PRIMARY KEY AUTOINCREMENT,
                        name TEXT NOT NULL,
                        price REAL NOT NULL,
                        description TEXT
                    )''')
    conn.commit()
    conn.close()

# Home route displaying products
@app.route('/')
def index():
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM products')
    products = cursor.fetchall()
    conn.close()
    return render_template('index.html', products=products)

# Route for individual product pages
@app.route('/product/<int:product_id>')
def product(product_id):
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM products WHERE id=?', (product_id,))
    product = cursor.fetchone()
    conn.close()
    return render_template('product.html', product=product)

# Add to cart route
@app.route('/add_to_cart', methods=['POST'])
def add_to_cart():
    product_id = request.form.get('product_id')
    # Logic to add product to cart (could use session, or a database table)
    return redirect(url_for('cart'))

# Cart page route
@app.route('/cart')
def cart():
    # Logic to display cart items
    return render_template('cart.html')

if _name_ == '_main_':
    init_db()  # Initialize the database
    app.run(debug=True)

    HTML CODE

    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>E-Commerce Website</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/styles.css') }}">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        <h1 class="mt-4">Welcome to Our E-Commerce Store</h1>
        <div class="row">
            {% for product in products %}
            <div class="col-md-4">
                <div class="card mb-4">
                    <img src="https://via.placeholder.com/150" class="card-img-top" alt="{{ product[1] }}">
                    <div class="card-body">
                        <h5 class="card-title">{{ product[1] }}</h5>
                        <p class="card-text">{{ product[3] }}</p>
                        <p class="card-text"><strong>${{ product[2] }}</strong></p>
                        <a href="{{ url_for('product', product_id=product[0]) }}" class="btn btn-primary">View Product</a>
                    </div>
                </div>
            </div>
            {% endfor %}
        </div>
    </div>
</body>
</html>
