# ed-laravel11-new
Whats new in Laravel 11

## No Http/Middleware folder
In older Laravel versions there was a Http/Middleware folder with some default middlewares such as:

- EncryptCookies
- TrimStrings
- RedirectIfAuthenticated
- etc

Now they are part of the framework instead of being in the Http/Middlewarefolder in your project. Of course, when you create a new middleware with the php artisan make:middleware command, it will still be created in the Http/Middleware folder.

## No Http/Kernel.php file

So you created a new Middlaware and you want to add it to your API routes. You would go to the Http/Kernel.php file just to realize it's not there anymore.

Now all Middlware configuration is done in the bootstrap/app.php file:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/ad02be27-684c-437d-b2e5-9ad034afd440)

As you can see, there's a new withMiddleware method where you can configure your middlewares.

## No RouteServiceProvider.php file

By default, Laravel 11 will not have a RouteServiceProvider.php file. So how can we configure our routes?

Let's say we want to add API routes with a prefix of /api. In previous Laravel versions this was done in the RouteServiceProvider.php file:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/d5ab13d0-7476-4543-b68a-711db29a6473)

ow in Laravel 11, we need to use the bootstrap/app.php file again

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/ae03f1df-76b6-411f-a8fb-70a88e9431af)

## No Console/Kernel.php file

In previous Laravel versions commands were loaded in the Console/Kernel.php file. This was also the place to write our task scheduling logic.

Now, in Laravel 11 task scheduling can be done in the routes/console.phpfile:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/f8a00419-3d4b-455f-85c0-d87e2d3bcf8a)

We can use the same methods for scheduling:

- call to schedule a callback
- command to schedule a Command
- job to schedule a Job

## Less config files by default

In a new Laravel 11 project we have less config files by default in the configdirectory. Some of them lives inside the framework, and they get merged, so we only need to add the ones we want to override.

These are the config files included by default:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/1fa4a41a-518a-49dc-b4a5-975116ef9d89)

If you need some of the older configs, such as broadcasting.php you can publish it by running:

```php artisan config:publish broadcasting```

And now you have the broadcasting.php file in your config directory.

​
## ​Eager load limit

I think this is the best new feature in Laravel 11. We can limit the number of records loaded when using eager load:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/6f89d493-e9b1-4dec-a1d9-e27eeb497ae8)

This will load the 10 most recent comments for each post. It's a pretty handy feature as it helps you avoid performance issues.

You can do the same in earlier Laravel versions as well, but you need to install the staudenmeir/eloquent-eager-limit package. In fact, this exact package was merged into Laravel 11.

## New casts method

In Laravel 11 there's a new method called casts that you can use to cast your model attributes. The old $casts property is still there, but the method will take precedence if both are present.

Since now it's a method we can do more stuff:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/b4e2a295-076e-4655-898d-ab225b20e686)

Although they are not new, but here are some interesting Model casts you might not know about:

- AsEnumCollection to store multiple enum values in a single column
- AsArrayObject to store and access an associative array easily since the standard array cast work only with primitive arrays
- AsCollection to cast values to a Laravel collection instead of a PHP array

## SQLite
The default database driver is now SQLite. I'm not sure if I love this change but of course it's pretty easy to change it to MySQL in the .env file.

## once

There's a new function called once which can be used to memoize functions.

Memoization is an optimization technique to speed up the execution of functions by caching the results of expensive calls and returning the cached result when the same inputs occur again. Basically, it's like a Redis a DB cache but for your functions.

Here's how memoization works:

- Function call: when a function is called with certain inputs, the function checks if the result for those inputs is already cached.
- Caching results: if the result is not cached, the function calculates the result and stores it in a data structure indexed by the input parameters.
- Returning cached result: on subsequent calls with the same inputs, instead of recalculating the result, the function retrieves the cached result from the data structure and returns it.

Memoization is particularly useful for optimizing recursive functions or functions with repeated computations. By storing previously computed results, it reduces redundant calculations and improves the performance of the program.

I don't think it's something that we use on a daily basis in high level web apps, but it's a nice addition to the framework.

The usage is pretty simple as always:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/3a842e6d-196d-4629-8c64-73aa556a2af6)

For this example, I added a var_dump statement to the bcrypt function to see how many times it's called.

Here's some code to test if it works:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/48276549-74ca-4658-8352-ddf3b61f7e5f)

And here's the output:

![image](https://github.com/GrytsenkoAndrey/ed-laravel11-new/assets/63291871/8890b711-c06b-4768-b5a7-7593e3bf619f)

As you can see, the bcrypt function is called only twice:

- Once with the input of password
- And one more time with the input of my-strong-pass

So all the subsequent calls with the same input will return the cached result.




