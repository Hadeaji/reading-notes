# Django CRUD and Forms

An HTML Form is a group of one or more fields/widgets on a web page,
which can be used to collect information from users for submission to a server.

Forms are a flexible mechanism for collecting user input because there are suitable widgets for entering many different types of data, including text boxes, checkboxes, radio buttons, date pickers and so on.

Working with forms can be complicated! Developers need to write HTML for the form, validate and properly sanitize entered data on the server (and possibly also in the browser), repost the form with error messages to inform users of any invalid fields, handle the data when it has successfully been submitted, and finally respond to the user in some way to indicate success.

> **Django Forms take a lot of the work out of all these steps** by providing a framework that lets you define forms and their fields programmatically,.

Then use these objects to both generate the form HTML code and handle much of the validation and user interaction.

## HTML Forms

First a brief overview of HTML Forms

The form is defined in HTML as a collection of elements inside `<form>...</form>` tags, containing at least one `input` element of `type="submit"`.

While here we just have one text field for entering the team name, a form may have any number of other input elements and their associated labels.

The field's type attribute defines what sort of widget will be displayed. The name and id of the field are used to identify the field in JavaScript/CSS/HTML, while value defines the initial value for the field when it is first displayed.

The submit input will be displayed as a button (by default) that can be pressed by the user to upload the data in all the other input elements in the form to the server.

The form attributes define the HTTP method used to send the data and the destination of the data on the server (action):

- **action:** The resource/URL where data is to be sent for processing when the form is submitted. If this is not set, then the form will be submitted back to the current page URL.
- **method:** The HTTP method used to send the data: post or get.
  - The `POST` method should always be used if the data is going to result in a change to the server's database because this can be made more resistant to cross-site forgery request attacks.
  - The `GET` method should only be used for forms that don't change user data (e.g. a search form). It is recommended for when you want to be able to bookmark or share the URL.

## Django form handling process

the view gets a request, performs any actions required including reading data from the models, then generates and returns an HTML page (from a template, into which we pass a context containing the data to be displayed).

What makes things more complicated is that the server also needs to be able to process data provided by the user, and redisplay the page if there are any errors.

the main things that Django's form handling does are:

- Display the default form the first time it is requested by the user.
  - The form may contain blank fields, or it may be pre-populated with initial values.
  - The form is referred to as unbound at this point, because it isn't associated with any user-entered data.
- Receive data from a submit request and bind it to the form.
  - Binding data to the form means that the user-entered data and any errors are available when we need to redisplay the form.
- Clean and validate the data.
  - Cleaning the data performs sanitization of the input and converts them into consistent Python types.
  - Validation checks that the values are appropriate for the field
- If any data is invalid, re-display the form, this time with any user populated values and error messages for the problem fields.
- If all data is valid, perform required actions
- Once all actions are complete, redirect the user to another page.

## Renew-book form using a Form and function view

### Form

The Form class is the heart of Django's form handling system. It specifies the fields in the form, their layout, display widgets, labels, initial values, valid values, and (once validated) the error messages associated with invalid fields.

#### Declaring a Form

The declaration syntax for a Form is very similar to that for declaring a Model, and shares the same field types (and some similar parameters). This makes sense because in both cases we need to ensure that each field handles the right types of data, is constrained to valid data, and has a description for display/documentation.

```
from django import forms

class RenewBookForm(forms.Form):
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")
```

#### Form fields

In this case, we have a single DateField for entering the renewal date that will render in HTML with a blank value, the default label "Renewal date:", and some helpful usage text: "Enter a date between now and 4 weeks (default 3 weeks)." As none of the other optional arguments are specified the field will accept dates using the input_formats: YYYY-MM-DD (2016-11-06), MM/DD/YYYY (02/26/2016), MM/DD/YY (10/25/16), and will be rendered using the default widget: DateInput.

There are many other types of form fields, which you will largely recognise from their similarity to the equivalent model field classes:
`BooleanField`, `CharField`, `ChoiceField`, `TypedChoiceField`, `DateField`, `DateTimeField`, `DecimalField`, `DurationField`, `EmailField`, `FileField`, `FilePathField`, `FloatField`, `ImageField`, `IntegerField`, `GenericIPAddressField`, `MultipleChoiceField`, `TypedMultipleChoiceField`, `NullBooleanField`, `RegexField`, `SlugField`, `TimeField`, `URLField`, `UUIDField`, `ComboField`, `MultiValueField`, `SplitDateTimeField`, `ModelMultipleChoiceField`, `ModelChoiceField`.

The arguments that are common to most fields are listed below (these have sensible default values):

- **required:** If True, the field may not be left blank or given a None value. Fields are required by default, so you would set required=False to allow blank values in the form.
- **label:** The label to use when rendering the field in HTML. If a label is not specified, Django will create one from the field name by capitalizing the first letter and replacing underscores with spaces (e.g. Renewal date).
- **label_suffix:** By default, a colon is displayed after the label (e.g. Renewal date:). This argument allows you to specify a different suffix containing other character(s).
- **initial:** The initial value for the field when the form is displayed.
- **widget:** The display widget to use.
- **help_text (as seen in the example above):** Additional text that can be displayed in forms to explain how to use the field.
- **error_messages:** A list of error messages for the field. You can override these with your own messages if needed.
- **validators:** A list of functions that will be called on the field when it is validated.
- **localize:** Enables the localization of form data input (see link for more information).
- **disabled:** The field is displayed but its value cannot be edited if this is True. The default is False.

### Validation

Django provides numerous places where you can validate your data. The easiest way to validate a single field is to override the method `clean_<fieldname>()` for the field you want to check.

```
import datetime

from django import forms
from django.core.exceptions import ValidationError
from django.utils.translation import ugettext_lazy as _

class RenewBookForm(forms.Form):
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")

    def clean_renewal_date(self):
        data = self.cleaned_data['renewal_date']

        # Check if a date is not in the past.
        if data < datetime.date.today():
            raise ValidationError(_('Invalid date - renewal in past'))

        # Check if a date is in the allowed range (+4 weeks from today).
        if data > datetime.date.today() + datetime.timedelta(weeks=4):
            raise ValidationError(_('Invalid date - renewal more than 4 weeks ahead'))

        # Remember to always return the cleaned data.
        return data
```

### URL configuration

Before we create our view, let's add a URL configuration for the renew-books page. Copy the following configuration to the bottom of **locallibrary/catalog/urls.py**.

```
urlpatterns += [
    path('book/<uuid:pk>/renew/', views.renew_book_librarian, name='renew-book-librarian'),
]
```

The URL configuration will redirect URLs with the format `/catalog/book/<bookinstance_id>/renew/` to the function named `renew_book_librarian()` in views.py, and send the `BookInstance` id as the parameter named `pk`.

### View

As discussed in the Django form handling process above, the view has to render the default form when it is first called and then either re-render it with error messages if the data is invalid, or process the data and redirect to a new page if the data is valid.

For forms that use a POST request to submit information to the server, the most common pattern is for the view to test against the POST request type (if request.method == 'POST':) to identify form validation requests and GET (using an else condition) to identify the initial form creation request.

First, we import our form (RenewBookForm) and a number of other useful objects/methods used in the body of the view function:

- **get_object_or_404():** Returns a specified object from a model based on its primary key value, and raises an Http404 exception (not found) if the record does not exist.
- **HttpResponseRedirect:** This creates a redirect to a specified URL (HTTP status code 302).
- **reverse():** This generates a URL from a URL configuration name and a set of arguments. It is the Python equivalent of the url tag that we've been using in our templates.
- **datetime:** A Python library for manipulating dates and times.

### The template

Create the template referenced in the view `(/catalog/templates/catalog/book_renew_librarian.html)` and copy the code below into it:

```
{% extends "base_generic.html" %}

{% block content %}
  <h1>Renew: {{ book_instance.book.title }}</h1>
  <p>Borrower: {{ book_instance.borrower }}</p>
  <p{% if book_instance.is_overdue %} class="text-danger"{% endif %}>Due date: {{ book_instance.due_back }}</p>

  <form action="" method="post">
    {% csrf_token %}
    <table>
    {{ form.as_table }}
    </table>
    <input type="submit" value="Submit">
  </form>
{% endblock %}
```

Most of this will be completely familiar from previous tutorials. We extend the base template and then redefine the content block. We are able to reference `{{ book_instance }}` (and its variables) because it was passed into the context object in the `render()` function, and we use these to list the book title, borrower, and the original due date.

Inside the tags, we define the `submit` input, which a user can press to submit the data.
The `{% csrf_token %}` added just inside the form tags is part of Django's cross-site forgery protection.

#### Other ways of using form template variable

Using `{{ form.as_table }}` as shown above, each field is rendered as a table row. You can also render each field as a list item (using `{{ form.as_ul }}` ) or as a paragraph (using `{{ form.as_p }}`).

It is also possible to have complete control over the rendering of each part of the form, by indexing its properties using dot notation. So, for example, we can access a number of separate items for our renewal_date field:

- `{{ form.renewal_date }}`: The whole field.
- `{{ form.renewal_date.errors }}`: The list of errors.
- `{{ form.renewal_date.id_for_label }}`: The id of the label.
- `{{ form.renewal_date.help_text }}`: The field help text.

### Testing the page

```
{% if perms.catalog.can_mark_returned %}- <a href="{% url 'renew-book-librarian' bookinst.id %}">Renew</a>  {% endif %}
```

## ModelForms

Creating a Form class using the approach described above is very flexible, allowing you to create whatever sort of form page you like and associate it with any model or models.

However, if you just need a form to map the fields of a single model then your model will already define most of the information that you need in your form: fields, labels, help text and so on.

A basic `ModelForm` containing the same field as our original `RenewBookForm` is shown below. All you need to do to create the form is add `class Meta` with the associated `model` (`BookInstance`) and a list of the model `fields` to include in the form.

```
from django.forms import ModelForm

from catalog.models import BookInstance

class RenewBookModelForm(ModelForm):
    class Meta:
        model = BookInstance
        fields = ['due_back']
```
