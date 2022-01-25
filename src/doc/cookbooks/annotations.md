# Using Annotations

To make your tests less verbose and easier to read/write, **Phake** allow you to initialize your objects properties using annotations. To do so, you need to add the `Mock` annotation on your object properties you would like to mock and call `Phake::initAnnotations($object)`.

** Example **

=== "PHP8"
	```{.php hl_lines="3 8 11 14 19"}
	<?php

	use Phake\Mock;
	use PHPUnit\Framework\TestCase;

	class FooTest extends TestCase
	{
		#[Mock]
		private MyClass $typedMock;

		#[Mock(MyClass::class)]
		private $untypedMock;

		#[Mock(class: MyClass::class)]
		private $untypedMock2;

		public function setUp(): void
		{
			Phake::initAnnotations($this);
		}

		public function testInitAnnotations(): void
		{
			$this->assertInstanceOf(MyClass::class, $this->typedMock);
			$this->assertInstanceOf(MyClass::class, $this->untypedMock);
			$this->assertInstanceOf(MyClass::class, $this->untypedMock2);
		}
	}
	```

=== "PHP7"
	```{.php hl_lines="8 13 14 20"}
	<?php

	use PHPUnit\Framework\TestCase;

	class MyClassTest extends TestCase
	{
		/**
		 * @Mock
		 */
		private MyClass $typedMock;

		/**
		 * @Mock
		 * @var MyClass
		 */
		private $untypedMock;

		public function setUp(): void
		{
			Phake::initAnnotations($this);
		}

		public function testInitAnnotations(): void
		{
			$this->assertInstanceOf(MyClass::class, $this->typedMock);
			$this->assertInstanceOf(MyClass::class, $this->untypedMock);
		}
	}
	```

If you're on PHP8 but want to use the old PHP7 annotations, you can add this line in your bootstrap.

```php-inline
Phake\Annotation\MockInitializer::setDefaultReader(new Phake\Annotation\LegacyReader());
```
