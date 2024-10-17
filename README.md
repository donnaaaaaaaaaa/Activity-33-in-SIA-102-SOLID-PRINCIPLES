Activity 33 in SIA 102: SOLID PRINCIPLES

1. Single Responsibility Principle (SRP)
A class should have only one reason to change, meaning it should have only one responsibility.
Here, the UserService class is responsible for handling user data, while the UserController class handles HTTP requests related to users. This separation allows each class to have a single responsibility, making the code easier to maintain.in Real-World Use Case; In an e-commerce application, you might have separate services for managing orders, customers, and products, each with its own class that adheres to SRP.
from flask import Flask, jsonify
app = Flask(__name__)

class UserService:
    def get_user(self, user_id):
        # Logic to retrieve user from the database
        pass

class UserController:
    def __init__(self, user_service):
        self.user_service = user_service

    @app.route('/user/<int:user_id>', methods=['GET'])
    def get_user(user_id):
        user = self.user_service.get_user(user_id)
        return jsonify(user)

user_service = UserService()
user_controller = UserController(user_service)

2. Open/Closed Principle (OCP)
Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.
To adhere to the Open-Closed Principle, we can design our Flask API to be easily extensible without modifying existing code. We can use inheritance and interfaces to achieve this. For example, if we want to add a new authentication method, we can create a new class that inherits from a base authentication class or implements an authentication interface.

# authentication.py
class BaseAuthentication:
    def authenticate(self):
        raise NotImplementedError

class UsernamePasswordAuthentication(BaseAuthentication):
    def authenticate(self):
        # Username and password authentication logic here
        return 'Authenticated with username and password'

class TokenAuthentication(BaseAuthentication):
    def authenticate(self):
        # Token-based authentication logic here
        return 'Authenticated with token'

@app.route('/login', methods=['POST'])
def login():
    auth_method = TokenAuthentication()  # Can be dynamically changed based on requirements
    result = auth_method.authenticate()
    return result
    
3.Liskov Substitution Principle (LSP)
Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.
To follow the Liskov Substitution Principle, we ensure that derived classes can be used interchangeably with their base classes. In Flask, we can define base classes or interfaces and make sure that derived classes adhere to the same contract.

# database.py
class BaseDatabase:
    def fetch_data(self):
        raise NotImplementedError

class MySQLDatabase(BaseDatabase):
    def fetch_data(self):
        # MySQL-specific data retrieval logic here
        return 'Data fetched from MySQL database'

class PostgresDatabase(BaseDatabase):
    def fetch_data(self):
        # Postgres-specific data retrieval logic here
        return 'Data fetched from Postgres database'

@app.route('/data', methods=['GET'])
def get_data():
    db = PostgresDatabase()  # Can be dynamically changed based on requirements
    result = db.fetch_data()
    return result
    
4. Interface Segregation Principle (ISP):
No client should be forced to depend on methods it does not use. Interfaces belong to clients, not to hierarchies.

class NotificationInterface:
    def send_email(self, message):
        pass
    
    def send_sms(self, message):
        pass

# Better approach
class EmailSender:
    def send_email(self, message):
        pass

class SMSSender:
    def send_sms(self, message):
        pass
	
In a messaging system, separating responsibilities for email and SMS ensures that changes in one type of notification do not unnecessarily complicate the other.

5. Dependency Inversion Principle (DIP):
Abstractions should not depend upon details. Details should depend upon abstractions.

class UserRepository:
    def fetch_users(self):
        pass

class UserService:
    def __init__(self, user_repository: UserRepository):
        self.user_repository = user_repository
    
    def get_users(self):
        return self.user_repository.fetch_users()
	
By coding against interfaces (abstractions), such as in a web app where UserService depends on UserRepository, we can easily swap out the data source without altering the service logic.

Applying the SOLID principles in your Python Flask applications promotes greater flexibility, maintainability, and readability. By adhering to these guidelines, you can craft software that is easier to extend and modify while reducing the risk of introducing bugs.
