# Worldwide-escorts-
Here you get a discreet hookup.   Have fun. Our ladies are experienced masseuse,,,welcome y'all🤗
<!DOCTYPE html>
<html>
<head>
  <title>Member Signup</title>
</head>
<body>
  <h1>Sign Up</h1>
  <form id="signupForm">
    <input type="text" name="name" placeholder="Name" required>
    <input type="email" name="email" placeholder="Email" required>
    <input type="password" name="password" placeholder="Password" required>
    <input type="text" name="city" placeholder="City" required>
    <textarea name="bio" placeholder="Profile Description"></textarea>
    <button type="submit">Sign Up</button>
  </form>

<script>
document.getElementById("signupForm").addEventListener("submit", async (e) => {
  e.preventDefault();
  const formData = new FormData(e.target);
  const obj = {};
  formData.forEach((value,key) => obj[key]=value);
  
  const res = await fetch("http://localhost:3000/signup", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(obj)
  });

  const data = await res.text();
  alert(data);
});
</script>
</body>
</html>
/project-root
    /frontend
        index.html        <- Landing page
        signup.html       <- Escort/Member signup
        login.html        <- User login
        dashboard.html    <- Paid content (members only)
    /backend
        server.js         <- Node.js backend
    /database
        db.sqlite         <- Stores users, profiles, payments
<!DOCTYPE html>
<html>
<head>
  <title>Adult Directory</title>
</head>
<body>
  <h1>Welcome to Nairobi Companions Directory</h1>
  <a href="signup.html">Join as a member</a>
  <br>
  <a href="login.html">Login</a>
</body>
</html>
<!DOCTYPE html>
<html>
<head>
  <title>Login</title>
</head>
<body>
<h1>Login</h1>
<form id="loginForm">
  <input type="email" name="email" placeholder="Email" required>
  <input type="password" name="password" placeholder="Password" required>
  <button type="submit">Login</button>
</form>
index.html
login.html
dashboard.html
<!DOCTYPE html>
<html>
<head>
  <title>Members Site</title>
</head>
<body>
  <h1>Welcome</h1>
  <a href="login.html">Login</a>
</body>
</html>
<!DOCTYPE html>
<html>
<head>
  <title>Login</title>
</head>
<body>

<h2>Login</h2>

<form id="loginForm">
  <input type="email" name="email" placeholder="Email" required />
  <input type="password" name="password" placeholder="Password" required />
  <button type="submit">Login</button>
</form>

<script>
document.getElementById("loginForm").addEventListener("submit", async (e) => {
  e.preventDefault();

  const data = {
    email: e.target.email.value,
    password: e.target.password.value
  };

  const res = await fetch("https://YOUR_RENDER_URL/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(data)
  });

  const result = await res.json();

  if (result.success) {
    window.location.href = "dashboard.html";
  } else {
    alert("Login failed");
  }
});
</script>

</body>
</html>
<!DOCTYPE html>
<html>
<head>
  <title>Dashboard</title>
</head>
<body>
  <h1>Members Area</h1>
  <p>You are logged in.</p>
</body>
</html>
</head>
<body>
<h1>Welcome to the Members Area</h1>
<p>Only paid users see this content.</p>
<!-- You can later add listings dynamically here -->
</body>
</html>
const express = require("express");
const bodyParser = require("body-parser");
const sqlite3 = require("sqlite3").verbose();
const bcrypt = require("bcrypt");

const app = express();
app.use(bodyParser.json());

// Database setup
const db = new sqlite3.Database("./database/db.sqlite");
db.serialize(() => {
  db.run(`CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    email TEXT UNIQUE,
    password TEXT,
    city TEXT,
    bio TEXT,
    paid INTEGER DEFAULT 0
  )`);
});

// Signup
app.post("/signup", async (req,res)=>{
  const {name,email,password,city,bio,3photos} = req.body;
  const hashPass = await bcrypt.hash(password,8);
  db.run("INSERT INTO users (name,email,password,city,bio) VALUES (?,?,?,?,?)",
         [name,email,hashPass,city,bio],
         function(err){
    if(err) return res.send("Error: "+err.message);
    res.send("Signup successful, pending payment approval");
  });
});

// Login
app.post("/login", (req,res)=>{
  const {email,password} = req.body;
  db.get("SELECT * FROM users WHERE email=?", [email], async (err,row)=>{
    if(!row) return res.json({success:false});
    const match = await bcrypt.compare(password,row.password);
    if(match && row.paid===1){
      res.json({success:true});
    } else {
      res.json({success:false});
    }
  });
});

// Payment verification endpoint (Flutterwave webhook)
app.post("/verify-payment", (req,res)=>{
  // Here you'd handle Flutterwave webhook logic
  // Example: mark user as paid in DB
  const {email} = req.body;
  db.run("UPDATE users SET paid=1 WHERE email=?", [email]);
  res.send("Payment verified");
});

app.listen(3000,()=>console.log("Server running on port 3000"));
<script src="https://checkout.flutterwave.com/v3.js"></script>
<button onClick="pay()">Pay KES 100</button>

<script>
function pay(){
  FlutterwaveCheckout({
    flutterwave key: "flutterwave key",
    tx_ref: "TX"+Math.floor(Math.random()*10000),
    amount: 100 a week,400 a month
    currency: "KES",
    redirect_url: "dashboard.html"
  });
}
</script>
npm install express body-parser sqlite3 bcrypt
