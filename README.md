Django REST framework tutorial
************
https://www.django-rest-framework.org/ 
API Guide for full documentation
************
    runserver
    * bash: curl -H 'Accept: application/json; indent=4' -au admin:123456 http://127.0.0.1:8000/users/
    
    
    * sudo apt  install httpie
    * bash: http -a admin:password123 http://127.0.0.1:8000/users/
    
    Or directly through the browser, by going to the URL http://127.0.0.1:8000/users/...
    
**********
    pip install django
    pip install djangorestframework
    pip install pygments  # We'll be using this for the code highlighting
    
************
Serilizators
    
    >>> from snippets.models import Snippet
    >>> from snippets.serializers import SnippetSerializer
    >>> from rest_framework.renderers import JSONRenderer
    >>> from rest_framework.parsers import JSONParser
    
    >>> snippet = Snippet(code='foo = "bar"\n')
    >>> snippet.save()
    >>> snippet = Snippet(code='print("hello, world")\n')
    >>> snippet.save()
    
    # render the data into json.
    >>> serializer = SnippetSerializer(snippet)
    >>>  serializer.data
    {'id': 2, 'title': '', 'code': 'print("hello, world")\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
    >>> content = JSONRenderer().render(serializer.data)
    >>> content
    b'{"id":2,"title":"","code":"print(\\"hello, world\\")\\n","linenos":false,"language":"python","style":"friendly"}'

    # Deserialization
    >>> import io
    >>> stream = io.BytesIO(content)
    >>> data = JSONParser().parse(stream)
    >>> serializer = SnippetSerializer(data=data)
    >>> serializer.is_valid()
    True
    >>> serializer.validated_data
    OrderedDict([('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
    >>> serializer.save()
    <Snippet: Snippet object (3)>

    # serialize querysets instead of model instances
    >>> serializer = SnippetSerializer(Snippet.objects.all(), many=True)
    >>> serializer.data
    [OrderedDict([('id', 1), ('title', ''), ('code', 'foo = "bar"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 2), ('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 3), ('title', ''), ('code', 'print("hello, world")'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])]

    # model serializer
    >>> from snippets.serializers import SnippetSerializer
    >>> serializer = SnippetSerializer()
    >>> print(repr(serializer))
    SnippetSerializer():
        id = IntegerField(read_only=True)
        title = CharField(allow_blank=True, max_length=100, required=False)
        code = CharField(style={'base_template': 'textarea.html'})
        linenos = BooleanField(required=False)
        language = ChoiceField(choices=[('abap', 'ABAP'), ('abnf', 'ABNF'), ('ada', 'Ada'), ('adl', 'ADL'), ('agda', 'Agda'), 
                                        ('aheui', 'Aheui'), ('ahk', 'autohotkey'), ('alloy', 'Alloy'), ('ampl', 'Ampl'),  .............. ])
                                        
        style = ChoiceField(choices=[('abap', 'abap'), ('algol', 'algol'), ('algol_nu', 'algol_nu'), ('arduino', 'arduino'), ('autumn', 'autumn'), ('borland', 'borland'), ('bw', 'bw'), ('colorful', 'colorful'), ('default', 'default'), ('emacs', 'emacs'), ('friendly', 'friendly'), ('fruity', 'fruity'), ('igor', 'igor'), ('inkpot', 'inkpot'), ('lovelace', 'lovelace'), ('manni', 'manni'), ('monokai', 'monokai'), ('murphy', 'murphy'), ('native', 'native'), ('paraiso-dark', 'paraiso-dark'), ('paraiso-light', 'paraiso-light'), ('pastie', 'pastie'), ('perldoc', 'perldoc'), ('rainbow_dash', 'rainbow_dash'), ('rrt', 'rrt'), ('sas', 'sas'), ('solarized-dark', 'solarized-dark'), ('solarized-light', 'solarized-light'), ('stata', 'stata'), ('stata-dark', 'stata-dark'), ('stata-light', 'stata-light'), ('tango', 'tango'), ('trac', 'trac'), ('vim', 'vim'), ('vs', 'vs'), ('xcode', 'xcode')], default='friendly')

    # It's important to remember that ModelSerializer classes don't do anything particularly magical, they are simply a shortcut for creating serializer classes:
    #   An automatically determined set of fields.
    #   Simple default implementations for the create() and update() methods.

   
***********************
Requests and Responses

    request.POST  # Only handles form data.  Only works for 'POST' method.
    request.data  # Handles arbitrary data.  Works for 'POST', 'PUT' and 'PATCH' methods.
    return Response(data)  # Renders to content type as requested by the client.
    
    The @api_view decorator for working with function based views.
    The APIView class for working with class-based views.
    
    in app urls:    urlpatterns = format_suffix_patterns(urlpatterns)

    $ http http://127.0.0.1:8000/snippets/ Accept:application/json  # Request JSON
    $ http http://127.0.0.1:8000/snippets/ Accept:text/html         # Request HTML

    $ http http://127.0.0.1:8000/snippets.json  # JSON suffix
    $ http http://127.0.0.1:8000/snippets.api   # Browsable API suffix

    # POST using form data
    $ http --form POST http://127.0.0.1:8000/snippets/ code="print(123)"
    # POST using JSON
    $ http --json POST http://127.0.0.1:8000/snippets/ code="print(456)"

******************************
Class-based Views
    
    Class-based Views without mixins
    Class-based Views with mixins
    Class-based Views with generic class-based views

***************************
Authentication & Permissions

    In code:        
        Code snippets are always associated with a creator.
        Only authenticated users may create snippets.
        Only the creator of a snippet may update or delete it.
        Unauthenticated requests should have full read-only access.

    changes in model and:
            rm -f db.sqlite3
            rm -r snippets/migrations
            python manage.py makemigrations snippets
            python manage.py migrate
    
    $ http POST http://127.0.0.1:8000/snippets/ code="print(123)"

        {
            "detail": "Authentication credentials were not provided."
        }
        
    $ http -a admin:53402505 POST http://127.0.0.1:8000/snippets/ code="print(789)"
    success!
    $ curl -H 'Accept: application/json; indent=4' -au admin:53402505 http://127.0.0.1:8000/snippets/
    success!
    
**************************
Relationships & Hyperlinked APIs
    
    Endpoint for the root of API
    Endpoint for the highlighted snippets


        Hyperlinking our API
    Dealing with relationships between entities is one of the more challenging aspects of Web API design. There 
    are a number of different ways that we might choose to represent a relationship:
        Using primary keys.
        Using hyperlinking between entities.
        Using a unique identifying slug field on the related entity.
        Using the default string representation of the related entity.
        Nesting the related entity inside the parent representation.
        Some other custom representation.

    The HyperlinkedModelSerializer has the following differences from ModelSerializer:
        It does not include the id field by default.
        It includes a url field, using HyperlinkedIdentityField.
        Relationships use HyperlinkedRelatedField, instead of PrimaryKeyRelatedField.

*******************
Pagination 
    
    settings.py
    
    REST_FRAMEWORK = {
        'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
        'PAGE_SIZE': 10
    }

*********************
ViewSets & Routers

    REST framework includes an abstraction for dealing with ViewSets, that allows the developer to concentrate on 
    modeling the state and interactions of the API, and leave the URL construction to be handled automatically, based on common conventions.
    ViewSet classes are almost the same thing as View classes, except that they provide operations such as retrieve, 
    or update, and not method handlers such as get or put.
    A ViewSet class is only bound to a set of method handlers at the last moment, when it is instantiated into a set of views, 
    typically by using a Router class which handles the complexities of defining the URL conf for you.
        
        
    # Create a router and register our viewsets with it.
    router = DefaultRouter()
    router.register(r'snippets', views.SnippetViewSet)
    router.register(r'users', views.UserViewSet)
    
    
    # The API URLs are now determined automatically by the router.
    urlpatterns = [
        path('', include(router.urls)),
    ]
