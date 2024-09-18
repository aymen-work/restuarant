To help you solve this Django Developer exam, I will walk you through the approach and the coding requirements for each part. Let's break it down:

### Part 1: Django Application (30 Points)
1. **Login Page**
   - Use Django's form framework to create a login form.
   - When submitting the login form, send a POST request to the API (`https://fakestoreapi.com/auth/login`).
   - Handle the response from the API: If the response contains a token, save the token (possibly in session storage) and redirect the user to the home page. If the login fails, show an error message.
   - On the home page, display a logout button.

2. **Product Model**
   - Create a Django model named `Product` with the fields: `id`, `title`, `price`, `description`, `category`, and `image`.
   - Perform migrations to create the corresponding database table in MySQL or PostgreSQL.

3. **Fetching Products and Filling the Database**
   - Fetch products from the provided API (`https://fakestoreapi.com/products`) using a `GET` request.
   - Parse the JSON response and fill the database with the product data.
   - You can use Django’s `bulk_create` method for efficient insertion into the database.

4. **Django REST API Endpoints**
   - Create Django REST Framework (DRF) endpoints for CRUD operations on the `Product` model (Create, Read, Update, Delete).
   - Use DRF’s serializers to validate the input fields.
   - Implement token-based authentication (JWT) for all methods except reading (list and detail view).
   - To secure endpoints, use Django’s built-in authentication mixins and middleware, such as `IsAuthenticated`.

5. **Optional: Caching**
   - For caching, you can use Redis with Django. The `django-redis` library is commonly used for this.
   - Focus on caching read-heavy operations, such as fetching the product list, to improve performance.

### Part 2: FastAPI Project (40 Points)
1. **FastAPI Setup and Product Categories Model**
   - Initialize a FastAPI project.
   - Create a new model `Category` for the product categories.
   - Configure the database connection using SQLAlchemy or Pydantic for validation.

2. **CRUD API for Categories**
   - Implement CRUD operations for the `Category` model.
   - Use `async` and `await` for database operations to ensure asynchronous execution.

3. **Search and Filter**
   - Implement search functionality to retrieve products that have a price less than 50. You can add query parameters like `?price_lt=50` to filter the results.

4. **JWT Authentication in FastAPI**
   - Set up JWT authentication for the FastAPI project using a library like `fastapi-jwt-auth`.

5. **Optional: Redis Caching**
   - Add Redis caching for frequently accessed endpoints, like product lists or category data. You can use the `aioredis` package for asynchronous Redis interaction in FastAPI.

### Additional Requirements:
- **Docker and Docker Compose**: 
   - Write `Dockerfile` for both the Django and FastAPI services.
   - Create a `docker-compose.yml` file to run both services in a multi-container environment.
   
- **Bonus: Passport API with Encryption**:
   - Implement an API in FastAPI that encrypts the category query value and decrypts it on the server before returning the products of that category.
   - You can use libraries like `cryptography` for handling encryption and decryption securely.

### Code Outline for Part 1:

#### Models (`models.py`):
```python
from django.db import models

class Product(models.Model):
    title = models.CharField(max_length=255)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    description = models.TextField()
    category = models.CharField(max_length=255)
    image = models.URLField()

    def __str__(self):
        return self.title
```

#### Views (`views.py`):
```python
from django.shortcuts import render, redirect
import requests
from django.conf import settings
from django.contrib.auth.decorators import login_required

# Login View
def login_view(request):
    if request.method == "POST":
        username = request.POST['username']
        password = request.POST['password']
        response = requests.post('https://fakestoreapi.com/auth/login', json={
            'username': username,
            'password': password
        })
        if response.status_code == 200:
            request.session['token'] = response.json()['token']
            return redirect('home')
        else:
            context = {'error': 'Invalid credentials'}
            return render(request, 'login.html', context)
    return render(request, 'login.html')

@login_required
def home_view(request):
    # Home page with logout button
    return render(request, 'home.html')

def logout_view(request):
    request.session.flush()
    return redirect('login')
```

#### API View for Products (`api/views.py`):
```python
from rest_framework import viewsets
from rest_framework.permissions import IsAuthenticatedOrReadOnly
from rest_framework.authentication import JWTAuthentication
from .models import Product
from .serializers import ProductSerializer

class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    authentication_classes = [JWTAuthentication]
    permission_classes = [IsAuthenticatedOrReadOnly]
```

#### Serializer (`serializers.py`):
```python
from rest_framework import serializers
from .models import Product

class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = '__all__'
```

#### URLs (`urls.py`):
```python
from django.urls import path, include
from .views import login_view, logout_view, home_view
from rest_framework.routers import DefaultRouter
from .api.views import ProductViewSet

router = DefaultRouter()
router.register('products', ProductViewSet)

urlpatterns = [
    path('login/', login_view, name='login'),
    path('logout/', logout_view, name='logout'),
    path('', home_view, name='home'),
    path('api/', include(router.urls)),
]
```

---

### Part 2: FastAPI Project Example

```python
from fastapi import FastAPI, Depends
from sqlalchemy.orm import Session
from models import Product, Category
from database import get_db
from auth import get_current_user

app = FastAPI()

@app.get("/products")
async def get_products(price_lt: float = None, db: Session = Depends(get_db)):
    query = db.query(Product)
    if price_lt:
        query = query.filter(Product.price < price_lt)
    return query.all()
```

This provides a general approach and basic structure for solving the exam. Let me know if you need further clarification or specific sections expanded!
