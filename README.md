<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BloomHub</title>
    <style>
        /* General Styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
            width: 96%;
        }
        body {
            background: #f5f5f5;
            text-align: center;
        }
        .hidden { display: none; }
        .container {
            max-width: 400px;
            margin: auto;
            padding: 20px;
            background: white;
            border-radius: 10px;
            box-shadow: 0px 4px 8px rgba(0,0,0,0.1);
            transition: all 0.3s ease-in-out;
        }
        button {
            width: 100%;
            padding: 12px;
            margin-top: 10px;
            background: #a3a15d;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background: #8b8a4a;
        }
        input {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        /* Cart Styles */
        .cart-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            padding: 10px;
            background: #f9f9f9;
            border-radius: 5px;
        }
        .total-price {
            font-size: 18px;
            font-weight: bold;
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <!-- Welcome Page -->
    <div id="welcome-page" class="container">
        <img src="WhatsApp Image 2025-03-18 at 13.59.22_6f16f753.jpg" alt="Product">
        <h1>BloomHub</h1>
        <button onclick="navigateTo('sign-in-page')">Get Started</button>
    </div>

    <!-- Sign In Page -->
    <div id="sign-in-page" class="container hidden">
        <h2>Sign In</h2>
        <input type="email" placeholder="Email">
        <input type="password" placeholder="Password">
        <button onclick="navigateTo('store-page')">Sign In</button>
    </div>

    <!-- Store Page -->
    <div id="store-page" class="container hidden">
        <h2>Explore</h2>
        <input type="text" placeholder="Search">
        <div class="product-grid">
            <div class="product" onclick="navigateTo('product-display', 'Rose Bouquet', 50)">
                <img src="WhatsApp Image 2025-03-18 at 13.04.51_145caa88.jpg" alt="Plant">
                <p>Rose Bouquet - $50</p>
            </div>
        </div>
    </div>

    <!-- Product Display Page -->
    <div id="product-display" class="container hidden">
        <img src="WhatsApp Image 2025-03-18 at 13.04.51_145caa88.jpg" alt="Product">
        <h2 id="product-name"></h2>
        <p id="product-price"></p>
        <button id="add-to-cart" onclick="addToCart()">Add to Cart</button>
    </div>

    <!-- Cart Page -->
    <div id="cart-page" class="container hidden">
        <h2>Shopping Cart</h2>
        <div id="cart-items"></div>
        <button onclick="navigateTo('checkout-page')">Proceed to Checkout</button>
    </div>

    <!-- Checkout Page -->
    <div id="checkout-page" class="container hidden">
        <h2>Checkout</h2>
        <div id="checkout-cart-items"></div>
        <p class="total-price">Total: $<span id="total-price">0</span></p>
        <button onclick="navigateTo('order-status')">Confirm Order</button>
    </div>

    <!-- Order Status Page -->
    <div id="order-status" class="container hidden">
        <h2>Order Status</h2>
        <button onclick="navigateTo('payment-method')">Make Payment</button>
    </div>

    <!-- Payment Method Page -->
    <div id="payment-method" class="container hidden">
        <h2>Payment Method</h2>
        <div class="payment-card visa">
            <p>VISA</p>
            <p>**** **** **** 1234</p>
        </div>
        <div class="payment-card mastercard">
            <p>Mastercard</p>
            <p>**** **** **** 1234</p>
        </div>
        <button onclick="navigateTo('order-tracking')">Pay Now</button>
    </div>

    <!-- Order Tracking Page -->
    <div id="order-tracking" class="container hidden">
        <h2>Order Tracking</h2>
        <p>Your order is being processed and will arrive soon.</p>
        <button onclick="navigateTo('order-successful')">Track</button>
    </div>

    <!-- Order Successful Page -->
    <div id="order-successful" class="container hidden">
        <h2>Order Successful</h2>
        <button onclick="navigateTo('store-page')">Continue Shopping</button>
    </div>

    <script>
        
        // Cart array to store added products
        let cart = [];

        // Function to navigate to different pages
        function navigateTo(pageId, productName = '', productPrice = '') {
            document.querySelectorAll('.container').forEach(page => page.classList.add('hidden'));
            document.getElementById(pageId).classList.remove('hidden');

            // Populate product display page with dynamic content
            if (pageId === 'product-display') {
                document.getElementById('product-name').textContent = productName;
                document.getElementById('product-price').textContent = `$${productPrice}`;
                window.currentProduct = { name: productName, price: productPrice }; // Store current product info
            }

            // Display cart items on the checkout page
            if (pageId === 'checkout-page') {
                displayCartItemsForCheckout();
                calculateTotalPrice();
            }
        }

        // Add product to cart
        function addToCart() {
            if (window.currentProduct) {
                cart.push(window.currentProduct); // Add current product to cart
                alert(`${window.currentProduct.name} added to cart!`);
            }
            navigateTo('cart-page');
            displayCart();
        }

        // Display cart items on the cart page
        function displayCart() {
            const cartItemsDiv = document.getElementById('cart-items');
            cartItemsDiv.innerHTML = ''; // Clear existing cart items

            if (cart.length === 0) {
                cartItemsDiv.innerHTML = '<p>Your cart is empty.</p>';
            } else {
                cart.forEach(item => {
                    const cartItemDiv = document.createElement('div');
                    cartItemDiv.classList.add('cart-item');
                    cartItemDiv.innerHTML = `${item.name} - $${item.price}`;
                    cartItemsDiv.appendChild(cartItemDiv);
                });
            }
        }

        // Display cart items on the checkout page
        function displayCartItemsForCheckout() {
            const checkoutCartItemsDiv = document.getElementById('checkout-cart-items');
            checkoutCartItemsDiv.innerHTML = ''; // Clear existing cart items

            if (cart.length === 0) {
                checkoutCartItemsDiv.innerHTML = '<p>Your cart is empty.</p>';
            } else {
                cart.forEach(item => {
                    const cartItemDiv = document.createElement('div');
                    cartItemDiv.classList.add('cart-item');
                    cartItemDiv.innerHTML = `${item.name} - $${item.price}`;
                    checkoutCartItemsDiv.appendChild(cartItemDiv);
                });
            }
        }
          
        // Calculate total price of cart
        function calculateTotalPrice() {
            const total = cart.reduce((sum, item) => sum + parseFloat(item.price), 0);
            document.getElementById('total-price').textContent = total.toFixed(2);
        }
        navigateTo('welcome-page');
    </script>

</body>
</html>

