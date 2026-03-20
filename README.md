<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DML Ayurvedic Store</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

<style>
body {
  font-family: 'Poppins', sans-serif;
  margin: 0;
  background: #f7f9f5;
}

header {
  background: #2e7d32;
  color: white;
  padding: 15px;
  text-align: center;
}

header h1 { margin: 0; }

.container {
  padding: 15px;
}

.categories {
  display: flex;
  overflow-x: auto;
  gap: 10px;
  margin-bottom: 15px;
}

.categories button {
  padding: 8px 15px;
  border: none;
  background: #ddd;
  border-radius: 20px;
  cursor: pointer;
}

.products {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px,1fr));
  gap: 15px;
}

.product {
  background: white;
  padding: 10px;
  border-radius: 10px;
  text-align: center;
}

.product img {
  width: 100%;
  height: 120px;
  object-fit: cover;
  border-radius: 8px;
}

button.add {
  background: #4caf50;
  color: white;
  border: none;
  padding: 8px;
  margin-top: 5px;
  border-radius: 5px;
  cursor: pointer;
}

.cart-btn {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #2e7d32;
  color: white;
  padding: 12px;
  border-radius: 50%;
  cursor: pointer;
}

.cart {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
  max-height: 70%;
  overflow: auto;
  display: none;
  padding: 15px;
  border-top: 2px solid #ccc;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

input, textarea {
  width: 100%;
  padding: 8px;
  margin: 5px 0;
}
</style>
</head>

<body>

<header>
<h1>DML Ayurvedic Store</h1>
<p>Natural Ayurvedic Wellness for a Healthy Life</p>
</header>

<div class="container">

<div class="products" id="products"></div>

</div>

<div class="cart-btn" onclick="toggleCart()">
🛒 <span id="count">0</span>
</div>

<div class="cart" id="cart">
<h3>Your Cart</h3>
<div id="cartItems"></div>
<h4>Total: ₹<span id="total">0</span></h4>

<h3>Place Order</h3>
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST" onsubmit="prepareOrder()">
<input name="name" placeholder="Name" required>
<input name="phone" placeholder="Phone" required>
<textarea name="address" placeholder="Address" required></textarea>
<textarea name="notes" placeholder="Instructions"></textarea>

<input type="hidden" name="order" id="orderData">

<button type="submit">Place Order</button>
</form>

</div>

<script>

const products = [
{ id:1, name:"Ashwagandha Powder", price:250, img:"https://i.ibb.co/8x5hYpT/herb1.jpg"},
{ id:2, name:"Amla Juice", price:180, img:"https://i.ibb.co/xs1K1h0/herb2.jpg"},
{ id:3, name:"Neem Tablets", price:120, img:"https://i.ibb.co/0jqHpnp/herb3.jpg"},
{ id:4, name:"Tulsi Drops", price:150, img:"https://i.ibb.co/fx0zKcM/herb4.jpg"},
{ id:5, name:"Aloe Vera Gel", price:200, img:"https://i.ibb.co/0qN9yXr/herb5.jpg"},
{ id:6, name:"Triphala Powder", price:220, img:"https://i.ibb.co/VqM0J7v/herb6.jpg"},
];

let cart = JSON.parse(localStorage.getItem("cart")) || {};

function renderProducts(){
  let html="";
  products.forEach(p=>{
    html+=`
    <div class="product">
      <img src="${p.img}" loading="lazy">
      <h4>${p.name}</h4>
      <p>₹${p.price}</p>
      <button class="add" onclick="addToCart(${p.id})">Add</button>
    </div>`;
  });
  document.getElementById("products").innerHTML=html;
}

function addToCart(id){
  cart[id] = (cart[id] || 0) + 1;
  updateCart();
}

function updateCart(){
  localStorage.setItem("cart", JSON.stringify(cart));
  let count=0, total=0, html="";
  for(let id in cart){
    let p = products.find(x=>x.id==id);
    let qty = cart[id];
    count+=qty;
    total+=p.price*qty;
    html+=`
    <div class="cart-item">
      ${p.name} x ${qty} = ₹${p.price*qty}
      <button onclick="removeItem(${id})">❌</button>
    </div>`;
  }
  document.getElementById("count").innerText=count;
  document.getElementById("cartItems").innerHTML=html;
  document.getElementById("total").innerText=total;
}

function removeItem(id){
  delete cart[id];
  updateCart();
}

function toggleCart(){
  let c = document.getElementById("cart");
  c.style.display = c.style.display=="block"?"none":"block";
}

function prepareOrder(){
  let text="";
  for(let id in cart){
    let p=products.find(x=>x.id==id);
    text+=`${p.name} x ${cart[id]} = ₹${p.price*cart[id]}\n`;
  }
  document.getElementById("orderData").value=text;
  localStorage.removeItem("cart");
}

renderProducts();
updateCart();

</script>

</body>
</html>
