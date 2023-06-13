# Projeto-Brazino
from flask import Flask, render_template, request, redirect

app = Flask(__name__)

users = []
bets = []

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        users.append({'username': username, 'password': password})
        return redirect('/login')
    return render_template('register.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        for user in users:
            if user['username'] == username and user['password'] == password:
                # Login successful, redirect to betting page
                return redirect('/bet')
        return redirect('/login')
    return render_template('login.html')

@app.route('/bet', methods=['GET', 'POST'])
def bet():
    if request.method == 'POST':
        amount = float(request.form['amount'])
        bet_type = request.form['bet_type']
        # Process the bet and save it
        bets.append({'amount': amount, 'bet_type': bet_type})
        return redirect('/bet')
    return render_template('bet.html', bets=bets)

if __name__ == '__main__':
    app.run(debug=True)
