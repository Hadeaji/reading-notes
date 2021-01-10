# Permissions

Together with authentication and throttling, permissions determine whether a request should be granted or denied access.

Permission checks are always run at the very start of the view, before any other code is allowed to proceed. Permission checks will typically use the authentication information in the request.user and request.auth properties to determine if the incoming request should be permitted.

Permissions are used to grant or deny access for different classes of users to different parts of the API.

The simplest style of permission would be to allow access to any authenticated user, and deny access to any unauthenticated user. This corresponds to the IsAuthenticated class in REST framework.

## How permissions are determined

Permissions in REST framework are always defined as a list of permission classes.

Before running the main body of the view each permission in the list is checked. If any permission check fails an `exceptions.PermissionDenied` or `exceptions.NotAuthenticated` exception will be raised, and the main body of the view will not run.

## Object level permissions

REST framework permissions also support object-level permissioning. Object level permissions are used to determine if a user should be allowed to act on a particular object, which will typically be a model instance.

Object level permissions are run by REST framework's generic views when `.get_object()` is called. As with view level permissions, an `exceptions.PermissionDenied` exception will be raised if the user is not allowed to act on the given object.

For example:

```
def get_object(self):
    obj = get_object_or_404(self.get_queryset(), pk=self.kwargs["pk"])
    self.check_object_permissions(self.request, obj)
    return obj
```

## Setting the permission policy

The default permission policy may be set globally, using the DEFAULT_PERMISSION_CLASSES setting. For example.

```
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ]
}
```

If not specified, this setting defaults to allowing unrestricted access:

```
'DEFAULT_PERMISSION_CLASSES': [
   'rest_framework.permissions.AllowAny',
]
```

You can also set the authentication policy on a per-view, or per-viewset basis, using the APIView class-based views.

```
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
from rest_framework.views import APIView

class ExampleView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request, format=None):
        content = {
            'status': 'request was permitted'
        }
        return Response(content)
```

## API Reference

- **AllowAny**

The AllowAny permission class will allow unrestricted access, regardless of if the request was authenticated or unauthenticated.

- **IsAuthenticated**

The IsAuthenticated permission class will deny permission to any unauthenticated user, and allow permission otherwise.

This permission is suitable if you want your API to only be accessible to registered users.

- **IsAdminUser**

The IsAdminUser permission class will deny permission to any user, unless user.is_staff is True in which case permission will be allowed.

This permission is suitable if you want your API to only be accessible to a subset of trusted administrators.

- **IsAuthenticatedOrReadOnly**

The IsAuthenticatedOrReadOnly will allow authenticated users to perform any request. Requests for unauthorised users will only be permitted if the request method is one of the "safe" methods; GET, HEAD or OPTIONS.

This permission is suitable if you want to your API to allow read permissions to anonymous users, and only allow write permissions to authenticated users.

## DjangoModelPermissions

This permission class ties into Django's standard django.contrib.auth model permissions. This permission must only be applied to views that have a .queryset property set. Authorization will only be granted if the user is authenticated and has the relevant model permissions assigned.

- `POST` requests require the user to have the `add` permission on the model.
- `PUT` and PATCH requests require the user to have the `change` permission on the model.
- `DELETE` requests require the user to have the `delete` permission on the model.

## DjangoObjectPermissions

This permission class ties into Django's standard object permissions framework that allows per-object permissions on models. In order to use this permission class, you'll also need to add a permission backend that supports object-level permissions, such as django-guardian.

As with DjangoModelPermissions, this permission must only be applied to views that have a .queryset property or .get_queryset() method. Authorization will only be granted if the user is authenticated and has the relevant per-object permissions and relevant model permissions assigned.

- `POST` requests require the user to have the `add` permission on the model instance.
- `PUT` and PATCH requests require the user to have the `change` permission on the model instance.
- `DELETE` requests require the user to have the `delete` permission on the model instance.

## Custom permissions

To implement a custom permission, override BasePermission and implement either, or both, of the following methods:

- `.has_permission(self, request, view)`
- `.has_object_permission(self, request, view, obj)`

The methods should return True if the request should be granted access, and False otherwise.

If you need to test if a request is a read operation or a write operation, you should check the request method against the constant `SAFE_METHODS`, which is a tuple containing `'GET'`, `'OPTIONS'` and `'HEAD'`

For example:

```
if request.method in permissions.SAFE_METHODS:
    # Check permissions for read-only request
else:
    # Check permissions for write request
```

## Third party packages

- **DRF - Access Policy**

The Django REST - Access Policy package provides a way to define complex access rules in declarative policy classes that are attached to view sets or function-based views.

- **Composed Permissions**

The Composed Permissions package provides a simple way to define complex and multi-depth (with logic operators) permission objects, using small and reusable components.

- **REST Condition**

The REST Condition package is another extension for building complex permissions in a simple and convenient way. The extension allows you to combine permissions with logical operators.

- **DRY Rest Permissions**

The DRY Rest Permissions package provides the ability to define different permissions for individual default and custom actions. This package is made for apps with permissions that are derived from relationships defined in the app's data model.

- **Django Rest Framework Roles**

The Django Rest Framework Roles package makes it easier to parameterize your API over multiple types of users.

- **Django REST Framework API Key**

The Django REST Framework API Key package provides permissions classes, models and helpers to add API key authorization to your API. It can be used to authorize internal or third-party backends and services which do not have a user account.

- **Django Rest Framework Role Filters**

The Django Rest Framework Role Filters package provides simple filtering over multiple types of roles.

- **Django Rest Framework PSQ**

The Django Rest Framework PSQ package is an extension that gives support for having action-based permission_classes, serializer_class, and queryset dependent on permission-based rules.