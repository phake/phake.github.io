# Cheat sheet

## Creating Mocks

Mocking a simple class

```php-inline
$mock = Phake::mock(MyClassToMock::class);
```

Mocking an interface

```php-inline
$mock = Phake::mock(MyInterface::class);
```

Mocking multiple interfaces

```php-inline
$mock = Phake::mock([ MyInterface1::class, MyInterface2::class ]);
```

Create a partial Mock

```php-inline
$mock = Phake::partialMock(MyClassToMock::class, ...$constructorParams);
```

## Method stubbing

Stubbing a simple call to the `calledMethod()`

```php-inline
$mock = Phake::mock(MyClassToMock::class);
Phake::when($mock)->calledMethod('someParameter')->thenReturn('My return value');
```

Stubbing multiple calls to `calledMethod()` with different parameters

```php-inline
$mock = Phake::mock(MyClassToMock::class);
Phake::when($mock)->calledMethod('someParameter')->thenReturn('My return value');
Phake::when($mock)->calledMethod('someOtherParameter')->thenReturn('My other return value');
```

Stub calls to `calledMethod()` regardless of parameters.

```php-inline
$mock = Phake::mock(MyClassToMock::class);
Phake::when($mock)->calledMethod(Phake::anyParameters())->thenReturn($result);

// or the shorter version

Phake::when($mock)->calledMethod->thenReturn($result);
```

Stub call to `calledMethod()` using callback

```php-inline
$mock = Phake::mock(MyClassToMock::class);
Phake::when($mock)->calledMethod($param1)->thenReturnCallback(fn($x) => $x + 1);
```

Stub consecutive calls to `calledMethod()`

```php-inline
$mock = Phake::mock(MyClassToMock::class);
Phake::when($mock)->calledMethod()->thenReturn(10)->thenReturn(20)->thenReturn(30);
```

Throw an exception when `calledMethod()` is called

```php-inline
$mock = Phake::mock(MyClassToMock::class);
Phake::when($mock)->calledMethod()->thenThrow(new \Exception('Some exception'));
```

## Method Verifications (Spies)

Verify that the `calledMethod()` was called once without parameters

```php-inline
Phake::verify($mock)->calledMethod();
```

Verify that the `calledMethod()` was called once with a specific parameter

```php-inline
Phake::verify($mock)->calledMethod('param');
```

Verify that the `calledMethod()` was called twice without parameters

```php-inline
Phake::verify($mock, Phake::times(2))->calledMethod();
```

Verify that the `calledMethod()` was called at least twice without parameters

```php-inline
Phake::verify($mock, Phake::atLeast(2))->calledMethod();
```

Verify that the `calledMethod()` was called at most twice without parameters

```php-inline
Phake::verify($mock, Phake::atMost(2))->calledMethod();
```

Verify that the `calledMethod()` was never called without parameters

```php-inline
Phake::verify($mock, Phake::never())->calledMethod();
```

Verify that mock did not receive any interaction

```php-inline
Phake::verifyNoInteraction($mock);
```

Verify that calls were made in order

```php-inline
Phake::inOrder(
	Phake::verify($mock)->firstCall(),
	Phake::verify($mock2)->secondCall(),
	Phake::verify($mock)->yetAnOthercall()
);
```

Capture parameters

```php-inline
Phake::verify($mock)->calledMethod(Phake::capture($parameter1));
```

