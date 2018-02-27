# Backend-assesment-Task5
5. Implement "to-do list"  app in Flask(python).
   Create a "to-do list" to organize and prioritize your tasks.
●	Hit the gym
●	Movie time
●	Office work
●	Reading
●	Meet liaquat
●	Family time
  Constraints:
●	  Use Rest client for API, don’t need to design frontend (GUI) for app
●	  Cover all usecase for to-do list, like add and delete item




For this we have to install sqlite3,SQLAlchemy,flask-SQLAlchemy

todo.py

from flask import Flask, render_template, request, redirect, url_for
from flask_sqlalchemy import SQLAlchemy
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite://///home/attique/PycharmProjects/todo/todo.db'
db = SQLAlchemy(app)
class Todo(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    text = db.Column(db.String(200))
    complete = db.Column(db.Boolean)
@app.route('/')
def index():
    incomplete = Todo.query.filter_by(complete=False).all()
    complete = Todo.query.filter_by(complete=True).all()
    return render_template('index.html', incomplete=incomplete , complete = complete)
@app.route('/add' , methods=['POST'])
def add():
    todo = Todo(text=request.form['todoitem'], complete=False)
    db.session.add(todo)
    db.session.commit()
    return redirect(url_for('index'))
@app.route('/complete/<id>')
def complete(id):
    #return '<h1>{}</h1>'.format(id)
    todo = Todo.query.filter_by(id=int(id)).first()
    todo.complete = True;
    db.session.commit()
    return redirect(url_for('index'))
#delete
@app.route('/delete/<id>')
def delete(id):
    todo = Todo.query.filter_by(id=int(id)).first()
    #todo.delete = delete(id);
    #todo = Todo(text=request.form['todoitem'], complete=False)
    db.session.delete(todo)
    db.session.commit()
    return redirect(url_for('index'))
if __name__ == '__main__':
    app.run(debug=True)



index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Todo</title>
</head>
<body>
<h1>TODO LIST</h1>
<div>Add a new todo item:
    <form action="{{ url_for('add') }}" method="POST">
        <input type="text" name="todoitem">
        <input type="submit" value="Add item">
    </form>
</div>
<div>
    <h2>Incomplete Items</h2>
    <ul>
        {% for todo in incomplete %}
            <li style="font-size: 30pt ">{{ todo.text }}<a href="{{ url_for('complete',id=todo.id) }}"  style="font-size: 10pt ">(Mark as Complete)</a>   <a href="{{ url_for('delete',id=todo.id) }}"  style="font-size: 5pt ">(delete)</a> </li>
        {% endfor %}
    </ul>
    <h2>Completed Items</h2>
    <ul>
        {% for todo in complete %}
        <li  style="font-size: 30pt ">
            {{ todo.text }} <a href="{{ url_for('delete',id=todo.id) }}"  style="font-size: 5pt ">(delete)</a>
        </li>
        {% endfor %}
    </ul>
</div>
</body>
</html>












output :
for output with images and better understanding please refer to todo.docx

running the app






















all entries of to do list



marked first three entries as complete



deleting “movie time” from the completed list









Deleting “reading” from the Incomplete list


showing the remaining entries in Database (todo.db)


