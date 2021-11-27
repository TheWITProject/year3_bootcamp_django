# Prework 4.1
Since the formatting was not maintained in google classroom, please follow the directions outlined below.

## Steps:
1. Install DRF using pip in your virtual environment. DRF is the Django REST Framework. Run this in your terminal: `pip3 install djangorestframework`
2. Create a new file called serializers.py in blog/. This is where you’ll tell Django how to serialize the models in models.py. The serializers will convert the Post model into JSON that will be used by the API to return data to the user. Add this to your serializers.py file:
```
from rest_framework import serializers
from blog.models import Post
class PostSerializer(serializers.ModelSerializer):
  class Meta:
    model = Post
      fields = ('author', 'title', 'text', 'created_date', ‘published_date’)
```
3. Update your blog/views.py file with a new view. You’ve created a view and URL that helps us navigate to the application from the browser. Now we’ll be creating a view and URL to the application’s API. In views.py, add a viewsets for your Post model. You’ll also need to import your serializer from step 2 to complete this step. Your blog/views.py file will look like this:
```
from django.shortcuts import render
from rest_framework import viewsets

from blog.serializers import PostSerializer
from blog.models import Post

class PostViewSet(viewsets.ModelViewSet):
  queryset = Post.objects.all()
  serializer_class = PostSerializer

def post_list(request):
  return render(request, 'blog/post_list.html', {}) 
```
4. Update your blog/urls.py file with a new URL. Once you’ve defined your viewset in step 3, you’re going to use the router functionality provided by DRF to route a desired API endpoint to the given viewset. Your file should now look like this:
```
from django.urls import include, path
from rest_framework import routers
from . import views
from blog.views import PostViewSet

router = routers.DefaultRouter()
router.register(r'post', PostViewSet)

urlpatterns = [
  path('', include(router.urls)),
  path('', views.post_list, name='post_list'),
]
```
5. Update your mysite/urls.py file. You will connect the main Django URL at mysite/urls.py to point to the app’s URL file. Your file should look like this: 
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
  path('admin/', admin.site.urls),
  path('', include('blog.urls')),
  path('api/', include('blog.urls')),
]
```
6. Update your mysite/settings.py file to include `rest_framework` in `INSTALLED_APPS`
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
    'rest_framework'
]
```
7. Test your API! Startup your server and navigate to the URL you created from your browser to see the API console. You can also make a GET request to your API from Postman.
Go to http://127.0.0.1:8000/api/post/ URL now, we should be able to use the browsable api to post our blog.
8. Create a new blog post. Try out a post request using your new API using Postman!
