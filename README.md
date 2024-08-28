

 @app.route("/")
 def hello_world():
    return "<p><b>Hello, World Test !</b></p>"



 @app.route("/home")
 def Home():
    return "<p> <b>This is the Home Page.!</b></p>"



 @app.route("/contact")
 def Contact():
    return "<p> <b>Phone no.:</b> 9149-----.</p> <p><b>Email :</b> test123@gmail.com</p> </p> <p><b>Name :</b> Deepak Jadon.</p>"  



 @app.route("/if-elif-else")
 def conditionalRendering():
    value = 1
     if value == 2:
        return "<p><b>Value is 2</b></p>"
     elif value < 2:
         return "<p><b>Value less than 2</b></p>"
     else:
        return "<p><b>Value is more than 2</b></p>"
    


 @app.route("/inline-if")
 def inlineIf():
    value = 3   
     msg = "The value is equal to 1"if value == 1 else "The value is not equal to 1"
    return "<p><b>{}</b></p>".format(msg)


 @app.route("/for-loop")
 def forLoop():
    marks =[1,2,3,4,5,6,7,8,9,10]
     totalMarks = 0
    for mark in marks:
        totalMarks = totalMarks + mark
     return {
        "status": 1,
         "message" : "success",
         "payload":
            {"totalMarks": totalMarks ,
            "value" : marks
         }}



 @app.route("/while-loop")
 def whileLoop():
    Value = 1
     counts = []
     while Value <= 15:
         counts.append(Value)
         Value = Value + 1
        return {
            "status": 1,
            "message" : "success",
           "payload":{
                'counts': counts,
                'value' : Value
                }
         }
    
    

 @app.route("/do-while-loop")
 def doWhileLoop():
     Value = 10
     counts = []
    while True:
         counts.append(Value)
         Value = Value + 1
         if Value > 15:
            break
         return {
             "status": 1,
             "message" : "success",
            "payload":{
                 'counts': counts,
                 'value' : Value
                 }
         }
    


 def addition(arg1,arg2):
     return arg1+arg2
@app.route("/function")
 def functionInPython():
    value = addition (10,20)
     return {
         "status": 1,
         "message" : "success",
         "payload":{
            'value' : value
             }
             }

   
 def multiply(arg1,arg2):
     print('arg1: ',arg1)
    print('arg2: ',arg2)
    value = arg1 * arg2
     return value
 @app.route("/function1")
def functionInPython1():
     value = multiply (arg1=10 ,arg2=20)
     return {
         "status": 1,
         "message" : "success",
         "payload":{
            'value' : value
             }
    }



 @app.route("/function2")
 def functionInPython2():
    value = [1,2,3,4]
     return {
         "status": 1,
         "message" : "success",
         "payload":{
             "value : the user name  is ":value
            }
     }


from flask import Flask,request
from pymongo import MongoClient


from flask_cors import CORS   #pip install flask_cors

app = Flask(__name__)

cors=CORS(app,resources={r"/*":{"origins":"*"}})

client=MongoClient('mongodb://localhost:27017/')

mdb=client['pro456']

collection = mdb['users']
@app.route("/db/insert")
def dataBase1():
    users =[
    {
        '_id' : 1,
        'name' : "John",
        'dept' : "CSE"
        },
        {
         '_id' : 2,
        'name' : "Rohan",
       'dept' : "CSE"
        },
        {
         '_id' : 3,
        'name' : "Jack",
        'dept' : "CSE"
        },
        {
         '_id' : 4,
        'name' : "Mohan",
       'dept' : "CSE"
        },
        {
         '_id' : 5,
        'name' : "Jojo",
        'dept' : "CSE"
        }
    ]
    collection.insert_many(users)
    return "database inserted"

@app.route("/db/read")
def read():
    userOne = mdb.users.find_one({'_id' : 1})
    users = mdb.users.find({'dept' : "CSE"})
    foundList = []
    for user in users:
        foundList.append(user)
    secondList = []
    for user in foundList:
        secondList.append(user)
    print ('users', users)
    return{'fountList' : foundList, "secondList": secondList, "userOne": userOne}



from flask import jsonify
@app.route("/db/update")
def update():
    user = mdb.users.find_one({'_id': 1})
    updatedUser = mdb.users.update_one({"_id": 1}, {"$set": {"location": "Naurangabad , GT Road"}})
    updateUser= mdb.users.update_one({'_id':1},{'$set':{'email':"thakurlakshya@gmail.com","password":'@dj9149370081'}} )
    return jsonify({"user": user, "updatedUser": {"matched_count": updatedUser.matched_count, "modified_count": updatedUser.modified_count}})




@app.route("/db/remove")
def remove():
    mdb.users.delete_one({"_id": 2})
    return{
        'status':1,
        'message':'user deleted',
        'class': "success"
    }

@app.route("/login", methods=['POST'])
def login1():
    input = request.json
    email = input.get('email')
    password = input.get('password')

    user = mdb.users.find_one({'email': email, 'password': password})

    if user and '_id' in user:
        return {
            'status': 1,
            'msg': "user exits correct detail",
            'class': "success"
        }
    else:
        return {
            'status': 0,
            'msg': "invalid credentials",
            'class': "error"
        }
    







