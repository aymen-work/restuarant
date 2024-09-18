

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
