[PHP](https://www.php.net) - very underappreciated,  server-side scripting language that's been backbone of the web from the beginning. Thanks to rich history combined with modern features (new releases fixed a lot of problems for which it got it's bad reputation - which persists, even today) it's one of best choices to learn as backend developer - lot of job opportunities in legacy projects as well as very quick development of greenfield projects using existing solutions that are battle tested. PHP is known for its simplicity and flexibility, and it is used by millions of websites worldwide.

[ORM](https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/) - In software development, an Object-Relational Mapping (ORM) is a technique that allows a program to interact with a relational database management system (RDBMS) using an object-oriented paradigm. An ORM library provides a set of classes and methods that map to the tables, rows, and columns of a database, making it possible to interact with the database using provided API. This means that developer can work with objects/classes rather than write raw SQL statements which can greatly enhance development speed and security (ORMs usually have build int input sanitization).

[Laravel](https://laravel.com) -  a web application framework with expressive, elegant syntax. It has rich ecosystem of libraries that allow us to create full stack web apps very efficiently. It strives to provide an amazing developer experience while providing powerful features such as thorough dependency injection, an expressive database abstraction layer, queues and scheduled jobs, unit and integration testing, and more.

[Laravel Breeze](https://laravel.com/docs/9.x/starter-kits#laravel-breeze) - is a minimal, simple implementation of all of Laravel's [authentication features](https://laravel.com/docs/9.x/authentication), including login, registration, password reset, email verification, and password confirmation. In addition, Breeze includes a simple "profile" page where the user may update their name, email address, and password.

[Laravel LiveWire](https://laravel-livewire.com) - is a library that makes it easy to build dynamic and interactive user interfaces using modern front-end JavaScript libraries such as Vue.js or React. It allows developers to create reactive components in PHP and render them on the server, and then automatically update the components in the browser when the underlying data changes. LiveWire uses its own reactivity system and "virtual DOM" (a lightweight in-memory representation of the DOM) to efficiently update the user interface without requiring a full page refresh (AJAX request to rerender components instead of page).

[Laravel Inertia](https://inertiajs.com) - Laravel Inertia is a JavaScript library that allows you to build single-page applications using server-side rendering with Laravel. It allows you to build SPAs in a way that feels like working with traditional server-rendered applications, while still getting the benefits of a SPA.

In Inertia, you define your application's pages as server-side templates, and then you use Inertia to render them on the client side. When you navigate to a new page, Inertia makes an HTTP request to the server to get the new template and data needed to render the page. It then seamlessly updates the page in the client without having to refresh the whole page. This allows you to build applications that feel fast and responsive, without having to worry about the complexity of client-side routing or state management.

[Laravel Tinker](https://laravel.com/docs/9.x/artisan#tinker) - allows you to interact with your entire Laravel application on the command line, including your Eloquent models, jobs, events, and more. To enter the Tinker environment, run:

```bash
php artisan tinker
```

---

# 🐛 Laravel

---

## Models

Models provide a powerful interface for you to interact with the tables in database. Laravel includes Eloquent, an object-relational mapper (ORM) that simplifies interactions with database. Each database table has a corresponding "Model" to interact with that table - in addition to retrieving records from the database table, models allow you to insert, update, and delete records from the table as well.

To create new model faster we can use:

```bash
php artisan make:model <ModelName>
```

To inspect models we can use:
```bash
php artisan model:show <ModelName>
```

We can add **--migration** modifier to migrate model straight away.

By convention, models should be kept in **app/Models** directory and tables created using migration will use **snake_case plural** form of model name.

Each model's corresponding database table has a primary key column named id. If necessary, you may define a protected **$primaryKey** property on your model to specify a different column that serves as your model's primary key. Primary key, by default, is an incrementing integer value, which means that Eloquent will automatically cast the primary key to an integer.

Each model is protected from mass assignment vulnerability because Laravel blocks mass assignment by default. Because they can be useful sometimes you can create **$fillable** field in your model to enable assignment of safe attributes:

```php
protected $fillable = [ 'attribute_name' ];
```

### Updating models

When we write update method in a controller (it receives **$request** and **$model** as arguments) we can first define rules for who can perform changes to db for given model by calling:

```php
$this->authorize('update', $model);
```

This will prevent anyone from updating given model until we define [Authorization Rules](https://laravel.com/docs/9.x/authorization) in our [Policy](https://laravel.com/docs/9.x/authorization#creating-policies) for given model.

Then, in the YourModelPolicy.php class we need to define a method that returns a bool:

```php
public function update(User $user, YourModel $model)
{
  return $model->user()->is($user);
}
```

[Read morea about models ->](https://laravel.com/docs/9.x/eloquent#introduction)

---

## Migrations

A version control for your database, allowing your team to define and share the application's database schema definition. Laravel's Schema facade provides database agnostic support for creating and manipulating tables across all of Laravel's supported database systems. To generate a database migration use:

```bash
php artisan migrate
```

*Each database migration will only be run once. To make additional to tables during development (update an un-deployed migration and rebuild your database from scratch) use **php artisan migrate:fresh**)*

or for single model:

```bash
php artisan make:migration <migration_name example: create_flights_table>
```

Laravel will use the name of the migration to attempt to guess the name of the table and whether or not the migration will be creating a new table. If Laravel is able to determine the table name from the migration name, Laravel will pre-fill the generated migration file with the specified table. Otherwise, you may simply specify the table in the migration file manually

Each new migration will be placed in your database/migrations directory. Each migration filename contains a timestamp that allows Laravel to determine the order of the migrations.

[Read more about Running Migrations ->](https://laravel.com/docs/9.x/migrations#running-migrations)

[Read more about Migrations ->](https://laravel.com/docs/9.x/migrations#introduction)

---

## Controllers

Controllers are responsible for processing requests made to your application and returning a response. All of request handling logic can be organized using "controller" classes. Controllers  group related request handling logic into a single class. For example, a UserController class might handle all incoming requests related to users, including showing, creating, updating, and deleting users. By default, controllers are stored in the **app/Http/Controllers** directory. all Controllers extend **App\Http\Controllers\Controller** class included in Laravel.

To create controller use:

```bash
php artisan make:controller <ControllerName>
```

Basic controller's usage:

```php
use App\Http\Controllers\UserController;

Route::get('/user/{is}', [UserController::class, 'show']);
```

### Resource Controllers

Each model in your application can be think of as a "resource" and since it is typical to perform CRUD operations on resources, Laravel resource routing assigns methods for those operations with a single line of code.

Example use:

```bash
php artisan make:controller <YourController> --resource
```

This command will generate a controller at **app/Http/Controllers/PhotoController.php**. The controller will contain a method for each of the available resource operations. Next, you may register a resource route that points to the controller:

```php
use App\Http\Controllers\PhotoController;

Route::resource('photos', PhotoController::class);
```

This single route declaration creates multiple routes to handle all CRUD operations. To view list of available routes use (add | grep <route-name> to filter):

```bash
php artisan route:list
```

![image info](./img/resource-routes.png)

[Reac more about Controllers ->](https://laravel.com/docs/9.x/controllers#introduction)

---

## Relationships

Relationships are defined as methods on your Eloquent model classes. Since relationships also serve as powerful [query builders](https://laravel.com/docs/9.x/queries), defining relationships as methods provides powerful method chaining and querying capabilities. Once the relationship is defined, we may retrieve the related record using Eloquent's dynamic properties. Dynamic properties allow you to access relationship methods as if they were properties defined on the model:

```php
$phone = User::find(1)->phone;
```

Foreign key of the relationship is determined based on the parent model name. Eloquent assumes that the foreign key should have a value matching the primary key column of the parent. In other words, Eloquent will look for the value of the user's **id**(primary key) column in the **user_id(foreign key column in phone table) column of the Phone record.


[Read more about Relationships ->](https://laravel.com/docs/9.x/eloquent-relationships#introduction)

---

## Rendering pages and Routing

### Using Inertia library

In Controllers render method return a component like so:

```php
use Inertia\Inertia;

// inside class:

public function index()
{
  return Inertia::render('PageName/Index', [
    'propName' -> $propValue,
    'secondPropName' -> $secondPropValue,
  ])
}
```

Where PageName and Index (case sensitive) is a a directory and file name that translates to: **resources/js/Pages/PageName/Index.vue**. In page component don't forget to receive props:

```ts
import { Head, Link } from '@inertiajs/inertia-vue3';

defineProps({
  propName: PropType,
  secondPropName: SecondPropType,
})
```

As you can see by the first line of example above, Inertia has some custom and ready to use components.

[Read more about Inertia ->](https://inertiajs.com)

If a route is simple we don't have to create controller for it. We can call Inertia directly when fetching route:

```php
Route::get('/', function () {
    return Inertia::render('Welcome');
})
```

Note that it is still considered best practice to handle rendering of any route in a dedicated controller class.

### Naming routes

You can always name a route which can be helpful if one controller handles multiple nested routes:

```php
Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
```

Then, in any method of any controller we can use name of the route in a function like so:

```php
 return Redirect::route('profile.edit');
```

### Route model binding

When injecting a model ID to a route or controller action, you often would have query the database to retrieve the model that corresponds to that ID in order to extract relevant data that you might want to render or work with. Laravel route model binding provides a convenient way to automatically inject the model instances directly into your routes.

```php
use App\Models\Model;

Route::get('/models/{model}', function (Model $model) {
    return Inertia:render('SomeModels/SampleModel', [
      'importantProp' -> $model->someRelevantAttribute
    ]);
});
```

In the example above we hint to Laravel that argument is of **Model** type and is named **$model** which also corresponds to variable part of the route( **{model}**). Thanks to this Laravel will retrieve and pass a whole model class instead of the id when querying this route. Implicit binding is also possible when using controller methods.

---

## Authentication

TODO

---
## Authorization (Gates and Policies)

Gates are simply functions that determine if a user is authorized to perform a given action. Typically, gates are defined within the boot method of the **App\Providers\AuthServiceProvider** class using the Gate facade.Using them we can check if action is allowed by calling **Gate::allows('rule-name', $args)** (or **Gate::denies**) in our route closure - if condition won't pass we can call **abort(403)**.

```php
Gate::define('update-post', function (User $user, Post $post) {
  return $user->id === $post->user_id;
});
```

Then in controller:

```php
if (! Gate::allows('update-post', $post)) {
  abort(403);
}
```

Even better practice is to use:

```php
Gate::authorize('update-post', $args);
```

This will throw if condition is not passed and Laravel will catch it and return 403 automatically. Gates always receive a user instance as their first argument and may optionally receive additional arguments (**$args** in example above) - a relevant model for example.

[Read more about Gates ->](https://laravel.com/docs/9.x/authorization#gates)

**Policies** are classes that organize authorization logic around a particular model or resource.

To create an empty Policy we can use artisan:

```bash
artisan make:policy ModelNamePolicy
```

*If you would like to generate a class with example policy methods related to viewing, creating, updating, and deleting the resource, you may provide a --model option when executing the command*

We can then define our policy as a method on that class and use it as rule name the Gate will use method on that policy for authorizing actions. This works because Laravel discovers policies automatically as long as they follow std convention (PostPolicy class located in Policies, PostModel located in Models and Post class as argument to Gate).
Returning custom responses from a policy is done using **Illuminate\Auth\Access\Response** class using allow or deny method.


You can also register policies manually if you want:

```php
protected $policies = [
  Post::class => EditableResourcePolicy::class,
];
```
[Read more about Policies ->](https://laravel.com/docs/9.x/authorization#creating-policies)

---

To do:
routes/api.php (API to communicate via JSON)
Ques
Notifications
Events + Listeners
