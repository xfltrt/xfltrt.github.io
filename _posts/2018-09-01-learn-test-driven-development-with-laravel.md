---
layout: post
published: true
category: development
title: Learn Test Driven Development with Laravel
---
I am building an URL shortener with extra features. Initially, I started building both front-end and back-end simultaneously. But as the scope grew too large for a side project, I started only building back-end as to not disturb the flow. That's when I started using Postman to send GET/POST requests.

For a while, this was fine until I grew tired of having to test the same thing over and over again. There has to be a better way? Fortunately, there is and it couldn't be any easier with PHPUnit which comes pre-packaged with Laravel (You still need to install PHPUnit on your computer though).

The Laravel Docs cover pretty much all you need to know. To start building your Test, you just need to run:

``` bash
php artisan make:test ThingToTest
```

This creates a file called ThingsToTest inside the `tests/features` folder. My application requires some *seeds* and I need to *refresh database* before running the test so I added these to my test file:

``` php
use RefreshDatabase;

public function setUp() : void
{
    parent::setUp();
    $this->artisan('db:seed');
}
```

This basically tells my Test to refresh migrations and seed database before running the test cases. I'm using the default database here but you can have a separate database for your test environment.

The generated test file already has an example in it so you can go ahead and type `phpunit` on your command line to start the Test. We haven't added any custom test so it should pass.

Now we can start making our own test cases/method. Here's an example of how I test if my URL has been shortened:

``` php
public function testCreateValidLink()
{
    // Perform Action
    $response = $this->json('POST', '/create/link', [
        'url' => 'https://dev.to/fitri',
        'name' => 'fitri'
    ]);

    // Test Result
    $response->assertJson(['created' => true]);
}
```

`/create/link` routes to my link controller Create method which returns a JSON object. In my case, the method returns an array with 'created' value set to either *true* or *false*.

I understand that I can use Faker to generate my *URL* and *name*. But my controller validates the URL and make sure it returns a 200/301/302 http_code. So I have to use a real URL in this case.

Each method is the test class/file represents a Test. Each method prefixed with 'assert' are assertions which returns either a pass or an error when you run `phpunit`. There are many kind of assert methods. These are the ones I've been using so far:

* $this->assertJson(['key' => 'value']);
* $this->assertDatabaseHas('table', ['field' => 'value']);
* $this->assertRedirect('url');

There's more listed in the official doc:

https://laravel.com/docs/5.6/http-tests#available-assertions
https://laravel.com/docs/5.6/database-testing#available-assertions

I've just learned everything last night but here are some useful tips I gathered from my trial and errors:

* Test file must end with Test (somethingTest.php). Otherwise, it won't be run by phpunit.
* Test case (methods) must start with test (testSomething()) or it will be skipped.
* alternatively you can add the comment /** @test */ before your method and name your method anything.
* Errors spewed from your tests are friends.

Of course, building a few tests aren't exactly TDD. But because I'm relying on Test classes instead of a front-end app. I have to either build my test or controller/methods first. Either way, they are not mutually exclusive.

Good luck!
