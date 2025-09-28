# all-in-one-delivery-service
all fresh items delivery at door step
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Fresh and Fast Delivery</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f8ff;
            color: #333;
            margin: 0; padding: 0;
        }
        header {
            background-color: #87ceeb;
            padding: 10px 20px;
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        header div.menu-item {
            background-color: white;
            color: #333;
            border: 2px solid #333;
            border-radius: 8px;
            padding: 8px 12px;
            cursor: pointer;
            margin-left: 10px;
        }
        header div.menu-item:hover {
            background-color: #b0e0f9;
        }
        main {
            padding: 20px;
        }
        .category-list {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        .category-list button {
            padding: 10px 15px;
            border: none;
            background-color: #87ceeb;
            color: white;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }
        .category-list button:hover {
            background-color: #00bfff;
        }
        .order-form {
            background: white;
            padding: 15px;
            border-radius: 10px;
            max-width: 600px;
            box-shadow: 0 0 10px #ccc;
        }
        .order-form label {
            display: block;
            margin-top: 10px;
            font-weight: bold;
        }
        .order-form input, 
        .order-form textarea {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border-radius: 5px;
            border: 1px solid #ccc;
            font-size: 1em;
        }
        .order-form button {
            background-color: #00bfff;
            color: white;
            border: none;
            margin-top: 15px;
            padding: 10px 20px;
            font-weight: bold;
            cursor: pointer;
            border-radius: 6px;
        }
        .order-form button:hover {
            background-color: #0077b6;
        }
        .hidden {
            display: none;
        }
        .info-display {
            margin-top: 20px;
            font-style: italic;
            color: #666;
        }
        .image-gallery {
            margin-top: 20px;
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
        }
        .image-gallery img {
            width: 120px;
            height: 100px;
            object-fit: cover;
            border-radius: 8px;
            border: 2px solid #ddd;
            cursor: pointer;
            transition: border-color 0.3s ease;
        }
        .image-gallery img:hover {
            border-color: #00bfff;
        }
    </style>
</head>
<body>
<header>
    <div class="menu-item" onclick="showHome()">Home</div>
    <div class="menu-item" onclick="openCamera()">Camera</div>
    <div class="menu-item" onclick="openGoogleMaps()">Google Maps</div>
    <div class="menu-item" onclick="showOrders()">Orders</div>
</header>
<main>
    <div id="homeScreen">
        <h1>Welcome to Grocery Ordering</h1>
        <div class="category-list">
            <button onclick="openCategory('groceries')">Groceries</button>
            <button onclick="openCategory('vegetables')">Vegetables</button>
            <button onclick="openCategory('milk')">Milk</button>
            <button onclick="openCategory('fruits')">Fruits</button>
            <button onclick="openCategory('flowers')">Flowers</button>
            <button onclick="openCategory('pharmacy')">Pharmacy</button>
            <button onclick="openCategory('food')">Food</button>
            <button onclick="openCategory('other')">Other</button>
        </div>
    </div>

    <div id="categoryScreen" class="hidden">
        <h2 id="categoryTitle"></h2>
        <div id="imageGallery" class="image-gallery"></div>

        <form id="orderForm" class="order-form">
            <label for="nameInput">Name:</label>
            <input type="text" id="nameInput" required />

            <label for="phoneInput">Phone Number:</label>
            <input type="tel" id="phoneInput" required pattern="\\d{10}" title="Enter 10 digit phone number" />

            <label for="orderItemInput">Order Item:</label>
            <textarea id="orderItemInput" rows="3" required></textarea>

            <label for="deliveryAddressInput">Delivery Address:</label>
            <textarea id="deliveryAddressInput" rows="3" required></textarea>

            <button type="submit">Place Order via WhatsApp</button>
        </form>
        <div id="infoDisplay" class="info-display"></div>
        <button onclick="goHome()" style="margin-top: 20px;">Back to Home</button>
    </div>

    <div id="ordersScreen" class="hidden">
        <h2>Your Orders</h2>
        <div id="ordersList">No orders placed yet.</div>
        <button onclick="goHome()" style="margin-top: 20px;">Back to Home</button>
    </div>
</main>

<script>
    let currentCategory = '';
    let orders = [];

    // Image URLs for each category; no folder needed as using public URLs
    const categoryImages = {
        groceries: [
            { src: 'https://via.placeholder.com/120x100?text=Rice', alt: 'Rice' },
            { src: 'https://via.placeholder.com/120x100?text=Wheat', alt: 'Wheat' }
        ],
        vegetables: [
            { src: 'https://via.placeholder.com/120x100?text=Tomato', alt: 'Tomato' },
            { src: 'https://via.placeholder.com/120x100?text=Potato', alt: 'Potato' }
        ],
        milk: [
            { src: 'https://via.placeholder.com/120x100?text=Milk+1L', alt: 'Milk 1 Litre' },
            { src: 'https://via.placeholder.com/120x100?text=Butter', alt: 'Butter' }
        ],
        fruits: [
            { src: 'https://via.placeholder.com/120x100?text=Mango', alt: 'Mango' },
            { src: 'https://via.placeholder.com/120x100?text=Banana', alt: 'Banana' }
        ],
        flowers: [
            { src: 'https://via.placeholder.com/120x100?text=Rose', alt: 'Rose' },
            { src: 'https://via.placeholder.com/120x100?text=Lily', alt: 'Lily' }
        ],
        pharmacy: [
            { src: 'https://via.placeholder.com/120x100?text=Tablet', alt: 'Tablet' },
            { src: 'https://via.placeholder.com/120x100?text=Syrup', alt: 'Syrup' }
        ],
        food: [
            { src: 'https://via.placeholder.com/120x100?text=Biriyani', alt: 'Biriyani' },
            { src: 'https://via.placeholder.com/120x100?text=Burger', alt: 'Burger' }
        ],
        other: [
            { src: 'https://via.placeholder.com/120x100?text=Custom+Item+1', alt: 'Custom Item 1' },
            { src: 'https://via.placeholder.com/120x100?text=Custom+Item+2', alt: 'Custom Item 2' }
        ]
    };

    function showHome() {
        document.getElementById('homeScreen').classList.remove('hidden');
        document.getElementById('categoryScreen').classList.add('hidden');
        document.getElementById('ordersScreen').classList.add('hidden');
        document.getElementById('orderForm').reset();
        document.getElementById('infoDisplay').innerText = '';
        currentCategory = '';
    }

    function openCategory(category) {
        currentCategory = category;
        document.getElementById('homeScreen').classList.add('hidden');
        document.getElementById('categoryScreen').classList.remove('hidden');
        document.getElementById('ordersScreen').classList.add('hidden');
        document.getElementById('categoryTitle').innerText = category.charAt(0).toUpperCase() + category.slice(1);
        document.getElementById('orderForm').reset();
        document.getElementById('infoDisplay').innerText = `Order your ${category} here.`;

        const gallery = document.getElementById('imageGallery');
        gallery.innerHTML = '';
        if(categoryImages[category]) {
            categoryImages[category].forEach(imgData => {
                const img = document.createElement('img');
                img.src = imgData.src;
                img.alt = imgData.alt;
                gallery.appendChild(img);

                img.addEventListener('click', () => {
                    document.getElementById('orderItemInput').value = imgData.alt;
                });
            });
        }
    }

    function goHome() {
        showHome();
    }

    document.getElementById('orderForm').addEventListener('submit', function(event) {
        event.preventDefault();
        const name = document.getElementById('nameInput').value.trim();
        const phone = document.getElementById('phoneInput').value.trim();
        const orderItem = document.getElementById('orderItemInput').value.trim();
        const address = document.getElementById('deliveryAddressInput').value.trim();
        if (!name || !phone || !orderItem || !address) {
            alert('Please fill all fields before submitting.');
            return;
        }
        if (!/^\d{10}$/.test(phone)) {
            alert('Phone number must be 10 digits.');
            return;
        }
        const message = `Name: ${name}%0APhone: ${phone}%0AOrder Item: ${orderItem}%0ADelivery Address: ${address}%0ACategory: ${currentCategory}`;
        const whatsappUrl = `https://wa.me/?text=${message}`;
        orders.push({ name, phone, orderItem, address, category: currentCategory, date: new Date().toLocaleString() });
        alert('Order placed successfully via WhatsApp.');
        window.open(whatsappUrl, '_blank');
        showHome();
    });

    function showOrders() {
        document.getElementById('homeScreen').classList.add('hidden');
        document.getElementById('categoryScreen').classList.add('hidden');
        document.getElementById('ordersScreen').classList.remove('hidden');
        let ordersList = document.getElementById('ordersList');
        if (orders.length === 0) {
            ordersList.innerText = 'No orders placed yet.';
            return;
        }
        ordersList.innerHTML = '';
        orders.forEach(order => {
            let orderDiv = document.createElement('div');
            orderDiv.style.border = '1px solid #ccc';
            orderDiv.style.padding = '10px';
            orderDiv.style.margin = '10px 0';
            orderDiv.innerHTML = `<strong>${order.category.toUpperCase()}</strong> - ${order.date}<br>Name: ${order.name}<br>Phone: ${order.phone}<br>Order: ${order.orderItem}<br>Address: ${order.address}`;
            ordersList.appendChild(orderDiv);
        });
    }

    function openCamera() {
        alert('Camera feature to be implemented as per device compatibility.');
    }

    function openGoogleMaps() {
        window.open('https://maps.google.com', '_blank');
    }

    showHome();
</script>
</body>
</html>
