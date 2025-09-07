# WB-Home
WB Home — Food delivery website demo (Pizza, Burger, Roll, Noodles) with login, cart, checkout, and location features.
<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>WB Home — Blinkit Style</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary: #e53935;
      --bg: #fafafa;
      --card: #ffffff;
      --muted: #6b7280;
      font-family: 'Inter', sans-serif;
    }
    * { box-sizing: border-box; }
    body { margin: 0; background: var(--bg); color: #111; }
    header { background: var(--primary); color: #fff; padding: 12px; text-align: center; font-weight: 600; }
    .hidden { display: none; }
    .login-card { max-width: 360px; margin: 50px auto; background: var(--card); padding: 20px; border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,.1); }
    .input { width: 100%; padding: 10px; margin: 6px 0; border: 1px solid #ddd; border-radius: 8px; }
    .btn { width: 100%; padding: 10px; background: var(--primary); color: #fff; border: none; border-radius: 8px; margin-top: 10px; cursor: pointer; }.products { display: grid; grid-template-columns: repeat(2,1fr); gap: 16px; padding: 16px; }
.product { background: var(--card); padding: 12px; border-radius: 12px; text-align: center; box-shadow: 0 2px 6px rgba(0,0,0,.05); }
.product img { width: 100px; height: 100px; border-radius: 50%; object-fit: cover; margin-bottom: 8px; }
.product h4 { margin: 0; font-size: 16px; }
.product p { margin: 4px 0; color: var(--muted); }
.add-btn { margin-top: 6px; padding: 6px 12px; border: 1px solid var(--primary); background: #fff; color: var(--primary); border-radius: 8px; cursor: pointer; font-size: 14px; }

.cart-btn { position: fixed; bottom: 20px; right: 20px; background: var(--primary); color: #fff; padding: 14px 20px; border-radius: 50px; font-weight: 600; box-shadow: 0 4px 10px rgba(0,0,0,.2); cursor: pointer; }

.checkout-card { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,.5); display:flex; align-items:center; justify-content:center; }
.checkout-inner { background: #fff; padding: 20px; border-radius: 12px; width: 90%; max-width: 400px; max-height: 90%; overflow:auto; }
.checkout-inner h3 { margin-top: 0; }
.small-btn { background:#0077cc; color:#fff; padding:6px 10px; border:none; border-radius:6px; margin:6px 0; cursor:pointer; font-size:14px; }
.map-link { font-size:13px; margin-top:4px; display:block; }

  </style>
</head>
<body>  <!-- Login Page -->  <div id="loginPage" class="login-card">
    <h2>WB Home</h2>
    <input id="phone" class="input" placeholder="Phone number" />
    <button class="btn" onclick="sendOTP()">Get OTP</button>
    <div id="otpSection" class="hidden">
      <input id="otp" class="input" placeholder="Enter OTP (1234)" />
      <button class="btn" onclick="verifyOTP()">Login</button>
    </div>
    <p style="text-align:center;color:var(--muted);margin-top:8px">Demo OTP = 1234</p>
  </div>  <!-- Main App -->  <div id="mainApp" class="hidden">
    <header>WB Home</header>
    <div class="products">
      <div class="product">
        <img src="https://images.unsplash.com/photo-1601924579440-0fef3f511dbc?w=400" alt="Pizza" />
        <h4>Pizza</h4>
        <p>₹199</p>
        <button class="add-btn" onclick="addToCart('Pizza',199)">Add</button>
      </div>
      <div class="product">
        <img src="https://images.unsplash.com/photo-1550547660-d9450f859349?w=400" alt="Burger" />
        <h4>Burger</h4>
        <p>₹99</p>
        <button class="add-btn" onclick="addToCart('Burger',99)">Add</button>
      </div>
      <div class="product">
        <img src="https://images.unsplash.com/photo-1601050690597-df65f3a6e4f1?w=400" alt="Roll" />
        <h4>Roll</h4>
        <p>₹79</p>
        <button class="add-btn" onclick="addToCart('Roll',79)">Add</button>
      </div>
      <div class="product">
        <img src="https://images.unsplash.com/photo-1627308595229-7830a5c91f9f?w=400" alt="Noodles" />
        <h4>Noodles</h4>
        <p>₹129</p>
        <button class="add-btn" onclick="addToCart('Noodles',129)">Add</button>
      </div>
    </div>
    <div id="cartBtn" class="cart-btn hidden" onclick="showCheckout()">Cart (0)</div>
  </div>  <!-- Checkout Modal -->  <div id="checkoutModal" class="checkout-card hidden">
    <div class="checkout-inner">
      <h3>Checkout</h3>
      <div id="cartSummary"></div>
      <h4>Delivery Details</h4>
      <input id="custName" class="input" placeholder="Full Name" />
      <input id="custPhone" class="input" placeholder="Phone Number" />
      <textarea id="custAddr" class="input" placeholder="Full Address"></textarea>
      <button class="small-btn" onclick="getLocation()">📍 Use Current Location</button>
      <div id="locationStatus" style="color:var(--muted);font-size:13px;margin-bottom:8px"></div>
      <h4>Payment Option</h4>
      <select id="payment" class="input">
        <option>Cash on Delivery</option>
        <option>Online Payment (Demo)</option>
      </select>
      <button class="btn" onclick="placeOrder()">Place Order</button>
      <button class="btn" style="background:#777;margin-top:6px" onclick="closeCheckout()">Cancel</button>
    </div>
  </div>  <!-- Thank You -->  <div id="thankYou" class="checkout-card hidden">
    <div class="checkout-inner">
      <h3>🎉 Thank You!</h3>
      <p>Your order has been placed successfully.</p>
      <button class="btn" onclick="closeThankYou()">Close</button>
    </div>
  </div>  <script>
    let cart = [];

    function sendOTP(){
      const phone = document.getElementById('phone').value.trim();
      if(phone.length<10){alert('Enter valid phone number');return;}
      document.getElementById('otpSection').classList.remove('hidden');
    }
    function verifyOTP(){
      const otp = document.getElementById('otp').value.trim();
      if(otp==='1234'){
        document.getElementById('loginPage').classList.add('hidden');
        document.getElementById('mainApp').classList.remove('hidden');
      }else{
        alert('Wrong OTP, try 1234');
      }
    }
    function addToCart(name, price){
      cart.push({name, price});
      document.getElementById('cartBtn').classList.remove('hidden');
      document.getElementById('cartBtn').innerText = `Cart (${cart.length})`;
    }
    function showCheckout(){
      let summary = cart.map(i=>`<p>${i.name} - ₹${i.price}</p>`).join("");
      let total = cart.reduce((a,b)=>a+b.price,0);
      document.getElementById('cartSummary').innerHTML = summary+`<h4>Total: ₹${total}</h4>`;
      document.getElementById('checkoutModal').classList.remove('hidden');
    }
    function closeCheckout(){
      document.getElementById('checkoutModal').classList.add('hidden');
    }
    function placeOrder(){
      const name = document.getElementById('custName').value.trim();
      const phone = document.getElementById('custPhone').value.trim();
      const addr = document.getElementById('custAddr').value.trim();
      if(!name || !phone || !addr){alert('Please fill all details');return;}
      document.getElementById('checkoutModal').classList.add('hidden');
      document.getElementById('thankYou').classList.remove('hidden');
    }
    function closeThankYou(){
      document.getElementById('thankYou').classList.add('hidden');
      cart=[];
      document.getElementById('cartBtn').classList.add('hidden');
    }

    function getLocation(){
      if(navigator.geolocation){
        document.getElementById('locationStatus').innerText = 'Fetching location...';
        navigator.geolocation.getCurrentPosition(showPosition, showError);
      }else{
        document.getElementById('locationStatus').innerText = 'Geolocation not supported';
      }
    }

    function showPosition(position){
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;
      document.getElementById('custAddr').value = `Lat: ${lat}, Lon: ${lon}`;
      const mapLink = `https://www.google.com/maps?q=${lat},${lon}`;
      document.getElementById('locationStatus').innerHTML = `📍 Location added! <a href="${mapLink}" target="_blank" class="map-link">[Open in Google Maps]</a>`;
    }

    function showError(error){
      switch(error.code){
        case error.PERMISSION_DENIED:
          document.getElementById('locationStatus').innerText = 'User denied location access';
          break;
        case error.POSITION_UNAVAILABLE:
          document.getElementById('locationStatus').innerText = 'Location unavailable';
          break;
        case error.TIMEOUT:
          document.getElementById('locationStatus').innerText = 'Request timed out';
          break;
        default:
          document.getElementById('locationStatus').innerText = 'Unknown error';
      }
    }
  </script></body>
</html>
