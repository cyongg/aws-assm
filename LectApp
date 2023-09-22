from flask import Flask, render_template, request
from pymysql import connections
import os
import boto3
from config import *

app = Flask(__name__)

bucket = custombucket
region = customregion

db_conn = connections.Connection(
    host=customhost,
    port=3306,
    user=customuser,
    password=custompass,
    db=customdb

)
output = {}
table = 'employee'


@app.route("/", methods=['GET', 'POST'])
def home():
    return render_template('Reg_Emp.html')


@app.route("/addemp", methods=['POST'])
def addlect():
    emp_id = request.form['emp_id']
    first_name = request.form['emp_first_name']
    last_name = request.form['emp_last_name']
    emp_position = request.form['emp_position']
    emp_password = request.form['emp_password']

    insert_sql = "INSERT INTO lecturer VALUES (%s, %s, %s, %s, %s)"
    cursor = db_conn.cursor()

    try:

        cursor.execute(insert_sql, (lect_id, first_name, last_name, lect_faculty, lect_password))
        db_conn.commit()

    finally:
        cursor.close()

    print("all modification done...")
    return render_template('Log_Lect.html')


@app.route("/login", methods=['POST'])
def login():
    lect_id = request.form['lect_id']
    lect_password = request.form['lect_password']

    # Perform database lookup to check credentials
    # Replace this with your actual database query logic
    cursor = db_conn.cursor()
    query = "SELECT * FROM lecturer WHERE lect_id = %s AND lect_password = %s"
    cursor.execute(query, (lect_id, lect_password))
    user = cursor.fetchone()
    cursor.close()

    if user:
        # Successful login
        return "Login Successful! Welcome"
    else:
        # Failed login
        return render_template('Log_Lect.html', error_message="Wrong username or password. Please try again.")

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=80, debug=True)
