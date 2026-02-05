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
