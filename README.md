# CodeAlpha_EcommerceStore
TASK 1 — Simple E-commerce Store

Create this GitHub repository first:

CodeAlpha_EcommerceStore


---

Full Project Structure

CodeAlpha_EcommerceStore/
│
├── backend/
│   ├── models/
│   │   ├── User.js
│   │   ├── Product.js
│   │   └── Order.js
│   │
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── productRoutes.js
│   │   └── orderRoutes.js
│   │
│   ├── server.js
│   ├── package.json
│   └── .env
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── script.js
│
├── README.md
└── screenshots/


---

1. Backend Setup

backend/package.json

{
  "name": "ecommerce-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "nodemon server.js"
  },
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "cors": "^2.8.5",
    "dotenv": "^16.4.5",
    "express": "^4.18.2",
    "jsonwebtoken": "^9.0.2",
    "mongoose": "^8.5.1",
    "nodemon": "^3.1.4"
  }
}


---

2. Install Packages

Open terminal:

cd backend
npm install


---

3. server.js

backend/server.js

const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");
require("dotenv").config();

const app = express();

app.use(cors());
app.use(express.json());

mongoose.connect(process.env.MONGO_URI)
.then(() => console.log("MongoDB Connected"))
.catch(err => console.log(err));

app.use("/api/auth", require("./routes/authRoutes"));
app.use("/api/products", require("./routes/productRoutes"));
app.use("/api/orders", require("./routes/orderRoutes"));

app.get("/", (req, res) => {
    res.send("E-commerce API Running");
});

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});


---

4. MongoDB Environment File

backend/.env

MONGO_URI=your_mongodb_connection
JWT_SECRET=codealpha_secret
PORT=5000


---

5. User Model

backend/models/User.js

const mongoose = require("mongoose");

const UserSchema = new mongoose.Schema({
    name: String,
    email: String,
    password: String
});

module.exports = mongoose.model("User", UserSchema);


---

6. Product Model

backend/models/Product.js

const mongoose = require("mongoose");

const ProductSchema = new mongoose.Schema({
    name: String,
    price: Number,
    image: String,
    description: String
});

module.exports = mongoose.model("Product", ProductSchema);


---

7. Order Model

backend/models/Order.js

const mongoose = require("mongoose");

const OrderSchema = new mongoose.Schema({
    userId: String,
    products: Array,
    total: Number
});

module.exports = mongoose.model("Order", OrderSchema);


---

8. Authentication Routes

backend/routes/authRoutes.js

const express = require("express");
const bcrypt = require("bcryptjs");
const jwt = require("jsonwebtoken");

const router = express.Router();
const User = require("../models/User");

router.post("/register", async (req, res) => {
    try {
        const { name, email, password } = req.body;

        const hashedPassword = await bcrypt.hash(password, 10);

        const user = new User({
            name,
            email,
            password: hashedPassword
        });

        await user.save();

        res.json({ message: "User Registered" });

    } catch (error) {
        res.status(500).json(error);
    }
});

router.post("/login", async (req, res) => {
    try {
        const { email, password } = req.body;

        const user = await User.findOne({ email });

        if (!user)
            return res.status(400).json({ message: "User not found" });

        const isMatch = await bcrypt.compare(password, user.password);

        if (!isMatch)
            return res.status(400).json({ message: "Invalid credentials" });

        const token = jwt.sign(
            { id: user._id },
            process.env.JWT_SECRET
        );

        res.json({ token });

    } catch (error) {
        res.status(500).json(error);
    }
});

module.exports = router;


---

9. Product Routes

backend/routes/productRoutes.js

const express = require("express");
const router = express.Router();

const Product = require("../models/Product");

router.get("/", async (req, res) => {
    const products = await Product.find();
    res.json(products);
});

router.post("/", async (req, res) => {
    const product = new Product(req.body);
    await product.save();

    res.json(product);
});

module.exports = router;


---

10. Order Routes

backend/routes/orderRoutes.js

const express = require("express");
const router = express.Router();

const Order = require("../models/Order");

router.post("/", async (req, res) => {
    const order = new Order(req.body);

    await order.save();

    res.json({
        message: "Order placed successfully"
    });
});

module.exports = router;


---

11. Frontend HTML

frontend/index.html

<!DOCTYPE html>
<html>
<head>
    <title>E-commerce Store</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<h1>Simple E-commerce Store</h1>

<div id="products"></div>

<script src="script.js"></script>
</body>
</html>


---

12. CSS

frontend/style.css

body {
    font-family: Arial;
    padding: 20px;
}

#products {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
}

.card {
    border: 1px solid #ccc;
    padding: 10px;
    width: 200px;
}


---

13. JavaScript Frontend

frontend/script.js

async function loadProducts() {

    const response = await fetch("http://localhost:5000/api/products");

    const products = await response.json();

    const productDiv = document.getElementById("products");

    products.forEach(product => {

        productDiv.innerHTML += `
            <div class="card">
                <img src="${product.image}" width="100%">
                <h3>${product.name}</h3>
                <p>${product.price}</p>
                <button>Add to Cart</button>
            </div>
        `;
    });
}

loadProducts();


---

14. README.md

README.md

# CodeAlpha E-commerce Store

## Features
- User Authentication
- Product Listing
- Shopping Cart
- Order Processing
- MongoDB Database

## Technologies Used
- HTML
- CSS
- JavaScript
- Node.js
- Express.js
- MongoDB

## Run Project

### Backend
cd backend
npm install
npm start

### Frontend
Open index.html


---

15. How to Run

Start Backend

cd backend
npm start

Open Frontend

Open:

frontend/index.html


---

16. GitHub Upload Commands

git init
git add .
git commit -m "Initial Commit"
git branch -M main
git remote add origin YOUR_GITHUB_REPO_LINK
git push -u origin main


---

17. Screenshots Needed

Add screenshots:

Home Page

Products

Cart

Login

MongoDB Database


Keep them inside:

/screenshots


---

18. Extra Features for Better Marks

Add: ✔ JWT Authentication
✔ Search Bar
✔ Responsive Design
✔ Add to Cart Counter
✔ Checkout Page
✔ Dark Mode

These help in getting better evaluation in CodeAlpha internship submissions.
