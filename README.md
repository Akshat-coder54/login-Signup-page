<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Login Popup Example</title>
  <style>
    body {
      margin: 1;
      padding:1;
      height: 100vh;
      background: linear-gradient(135deg, #264653, #2a9d8f, #e9c46a, #f4a261, #e76f51);
      background-size: 300% 300%;
      animation: bgmove 15s infinite alternate;
      font-family: "Poppins", sans-serif;
      overflow: hidden;
    }

    @keyframes bgmove {
      0% { background-position: left top; }
      100% { background-position: right bottom; }
    }

    #loginBtn {
      position: fixed;
      top: 15px;
      right: 13px;
      background: #2a9d8f;
      color: #fff;
      border: none;
      padding: 6px 18px;
      border-radius: 12px;
      cursor: pointer;
      font-size: 0.85rem;
      font-weight: 400;
      transition: 0.4s;
      z-index: 20;
    }

    #loginBtn:hover { background: #21867a; }

    .popup {
      display: none;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      padding: 25px;
      border-radius: 12px;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
      width: 320px;
      text-align: center;
      z-index: 30;
      opacity: 0;
      transition: opacity 0.3s ease;
    }

    .popup.active { display: block; opacity: 1; }

    .popup h3 { margin-bottom: 10px; color: #264653; }

    .popup input {
      width: 90%;
      padding: 8px;
      margin: 8px 0;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 0.9rem;
    }

    .popup button {
      width: 95%;
      background: #2a9d8f;
      color: white;
      border: none;
      padding: 8px;
      margin-top: 10px;
      border-radius: 8px;
      cursor: pointer;
      transition: 0.3s;
    }

    .popup button:hover { background: #21867a; }

    .switch { margin-top: 12px; font-size: 0.85rem; color: #333; }

    .switch a {
      color: #2a9d8f;
      text-decoration: none;
      font-weight: 500;
      cursor: pointer;
    }

    .switch a:hover { text-decoration: underline; }
  </style>
</head>
<body>

<button id="loginBtn">Login</button>

<!-- Login Box -->
<div id="loginPopup" class="popup">
  <h3>Login</h3>
  <input type="text" id="loginUser" placeholder="Email or User ID" required />
  <input type="password" id="loginPass" placeholder="Password" required />
  <button id="loginSubmit">Login</button>
  <div class="switch">
    Don’t have an account? <a onclick="toggleForm('signup')">Sign up</a>
  </div>
</div>

<!-- Signup Box -->
<div id="signupPopup" class="popup">
  <h3>Sign Up</h3>
  <input type="text" id="firstName" placeholder="First Name" required />
  <input type="text" id="middleName" placeholder="Middle Name" />
  <input type="text" id="lastName" placeholder="Last Name" required />
  <input type="email" id="email" placeholder="Email ID" required />
  <input type="password" id="password" placeholder="Password (min 8 chars, 1 symbol)" required />
  <button id="signupSubmit">Sign Up</button>
  <div class="switch">
    Already have an account? <a onclick="toggleForm('login')">Login</a>
  </div>
</div>

<script>
  const loginBtn = document.getElementById("loginBtn");
  const loginPopup = document.getElementById("loginPopup");
  const signupPopup = document.getElementById("signupPopup");
  let loggedIn = false;

  loginBtn.addEventListener("click", () => {
    if (loggedIn) {
      loggedIn = false;
      loginBtn.textContent = "Login";
    } else {
      showPopup(loginPopup);
    }
  });

  function showPopup(popup) {
    hideAll();
    popup.classList.add("active");
  }

  function hideAll() {
    loginPopup.classList.remove("active");
    signupPopup.classList.remove("active");
  }

  function toggleForm(type) {
    hideAll();
    if (type === "signup") showPopup(signupPopup);
    else showPopup(loginPopup);
  }

  document.getElementById("loginSubmit").addEventListener("click", handleLogin);
  document.getElementById("signupSubmit").addEventListener("click", handleSignup);

  document.addEventListener("keydown", (e) => {
    if (e.key === "Enter" && loginPopup.classList.contains("active")) handleLogin();
  });

  function handleLogin() {
    const user = document.getElementById("loginUser").value.trim();
    const pass = document.getElementById("loginPass").value.trim();
    if (user && pass) {
      loggedIn = true;
      loginBtn.textContent = "Logout";
    
      window.location.href = "obito.html";
    } else {
      alert("Please fill in both fields!");
    }
  }

  function handleSignup() {
    const f = document.getElementById("firstName").value.trim();
    const l = document.getElementById("lastName").value.trim();
    const e = document.getElementById("email").value.trim();
    const p = document.getElementById("password").value.trim();

    const passReq = /^(?=.*[!@#$%^&*])(?=.{8,})/;
    if (!f || !l || !e || !p) {
      alert("Please fill all required fields!");
      return;
    }
    if (!passReq.test(p)) {
      alert("Password must be at least 8 characters and include a symbol.");
      return;
    }
    hideAll();
    alert("Signup successful! Please login.");
    showPopup(loginPopup);
  }


  window.onclick = function(event) {
    if (event.target === loginPopup || event.target === signupPopup) hideAll();
  };
</script>
</body>
</html>
