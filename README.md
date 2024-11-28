# ALLERGAURD
PROBLEM STATEMENT

In today’s fast-paced world, individuals with allergies face significant challenges in identifying allergens in food and their environment. Food labels are often hard to read, and environmental allergens like pollen and pollution are difficult to track. Managing multiple allergies requires constant attention, making it easy for individuals to miss potential risks.
AllerGuard bridges this gap by offering a mobile app that uses image recognition, natural language processing, and real-time data to provide personalized, real-time allergy management tools, helping users avoid food and environmental allergens and stay safe.

BACKEND CODE:
from flask import Flask, render_template, request, redirect, url_for
import requests

app = Flask(__name__)

# Mock user allergen data
user_data = {"allergens": []}

# OpenFoodFacts API URL
FOOD_API_URL = "https://world.openfoodfacts.org/api/v0/product/"

# Routes
@app.route("/")
def home():
    return render_template("home.html")

@app.route("/profile", methods=["GET", "POST"])
def profile():
    if request.method == "POST":
        allergens = request.form.get("allergens").split(",")
        user_data["allergens"] = [a.strip().lower() for a in allergens]
        return redirect(url_for("scan"))
    return render_template("profile.html")

@app.route("/scan", methods=["GET", "POST"])
def scan():
    if request.method == "POST":
        barcode = request.form.get("barcode")
        response = requests.get(f"{FOOD_API_URL}{barcode}.json")
        
        if response.status_code == 200:
            product = response.json().get("product", {})
            product_allergens = product.get("allergens_tags", [])
            matched_allergens = [a for a in user_data["allergens"] if a in product_allergens]
            return render_template("result.html", allergens=matched_allergens, product=product.get("product_name", "Unknown"))
        
        return "Error: Product not found or API issue."
    return render_template("scan.html")

@app.route("/result")
def result():
    return render_template("result.html")

if __name__ == "__main__":
    app.run(debug=True)


FRONT END
HOME PAGE:
<!DOCTYPE html>
<html>
<head>
    <title>Allergy Alert</title>
</head>
<body>
    <h1>Welcome to Allergy Alert</h1>
    <a href="/profile">Get Started</a>
</body>
</html>


PROFILE PAGE:
<!DOCTYPE html>
<html>
<head>
    <title>Allergy Profile</title>
</head>
<body>
    <h2>Enter Your Allergens</h2>
    <form method="POST">
        <label for="allergens">Allergens (comma-separated):</label>
        <input type="text" name="allergens" required>
        <button type="submit">Save</button>
    </form>
</body>
</html>


SCANNER PAGE:
<!DOCTYPE html>
<html>
<head>
    <title>Scan Product</title>
</head>
<body>
    <h2>Scan a Product</h2>
    <form method="POST">
        <label for="barcode">Enter Barcode:</label>
        <input type="text" name="barcode" required>
        <button type="submit">Check</button>
    </form>
</body>
</html>



RESULT PAGE:
<!DOCTYPE html>
<html>
<head>
    <title>Results</title>
</head>
<body>
    <h2>Product Results</h2>
    {% if allergens %}
        <p>The product contains these allergens: {{ allergens }}</p>
    {% else %}
        <p>No allergens detected! This product is safe to consume.</p>
    {% endif %}
    <a href="/scan">Check Another Product</a>
</body>
</html>

PRESENTATION:
![1](https://github.com/user-attachments/assets/1de1429f-cff1-4bdc-ae24-6788fab979de)
![2](https://github.com/user-attachments/assets/5d2447c5-93fc-4bfe-b6a4-72f955b89e3a)
![3](https://github.com/user-attachments/assets/2087a8eb-aedd-4a4f-a1f3-59253b5c9473)
![4](https://github.com/user-attachments/assets/01506791-81bb-4b7f-8f74-bcf087cac33a)
![5](https://github.com/user-attachments/assets/958ca967-f2d7-4e90-a79e-d565f844ce57)
![6](https://github.com/user-attachments/assets/596954f5-a97e-451a-b15d-77740fa624e2)
![7](https://github.com/user-attachments/assets/ec343524-6cd0-4de7-be2f-e55814634ade)
![8](https://github.com/user-attachments/assets/0f86b2eb-d1f5-46e8-87b6-8ab2dcbfb014)
![9](https://github.com/user-attachments/assets/c02e0dd0-d51f-4e72-9b0e-05ade964700a)

OUTPUT:
Due to some technical and logical error, the required output couldn't be generated
The end product was to create an app that helps the local with their allergy issues guiding them through what foods they need to consume and what are the medical precautions they need to take incase a particular allergy occurs. Along with this the plan was to incorporate the app in helping people with their environmantal allergies such as pollen by mentioning the pollen level in their given location by integrating it with AI.








