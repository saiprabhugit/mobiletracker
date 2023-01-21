from flask import Flask, request
import mysql.connector

app = Flask(__name__)

@app.route('/')
def index():
    return '''
        <form method="POST" action="/search">
            Phone Number: <input type="text" name="phone_number"><br>
            <input type="submit" value="Search">
        </form>
    '''

@app.route('/search', methods=['POST'])
def search():
    phone_number = request.form['phone_number']
    conn = mysql.connector.connect(user='<username>', password='<password>',
                                  host='<hostname>', database='<dbname>')
    cursor = conn.cursor()
    query = 'SELECT * FROM phone_numbers WHERE number = %s'
    cursor.execute(query, (phone_number,))
    results = cursor.fetchall()
    if len(results) > 0:
        return f'Phone number {phone_number} already exists in the database.'
    else:
        return f'Phone number {phone_number} does not exist in the database.'

if __name__ == '__main__':
    app.run(debug=True)
