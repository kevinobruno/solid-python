# 03 Liskov Substitution Principle (LSP)

- Objects should be replaceable with their subtypes without affeting the correctness of the program.
- Change the "Is-A" way of thinking:
    - "If it looks like a duck and quacks like a duck but it needs batteries, you probably have
    the wrong abstraction".

## Breaking the hierarchy if it fails the substitution test

Imagine we have a class that abstract a `Car` with a method that returns the cabin width.

```python
class Car:

    @property
    def cabin_width(self) -> float:
        # Returns cabin width
        return 0.0
```

This class works with most common cars, but if we have a Formula 1 racing car inherit this class, we
would have this.

```python
class RacingCar(Car):

    @property
    def cookpit_width(self) -> float:
        # Returns cookpit width
        return 1.23
```

This means that Formula 1 racing cars does not have a cabin, they have a cookpit, so the function
`cabin_width` will not be returning the cabin width.

As `RacingCar` is a `Car`, we could think of use both classes function `cabin_width`.

```python
class CarUtils:

    def print_cabin_width(self):
        car_1 = Car()
        car_2 = Car()
        car_3 = RacingCar()

        cars = [car_1, car_2, car_3]

        for car in cars:
            print(car.cabin_width)
```

When running whe code above, there will be a wrong behavior when printing the cabin width of the
third car, because of the `cabin_width` property not implemented on `RacingCar` class.

To fix this, we need to break the hierarchy, turning both of our classes into a more generic class.
In our example it could be a `Vehicle`, that can be a car, a boat, an airplane, etc, then both of
our classes can inherit from the more generic class.

```python
class Vehicle:

    @property
    def interior_width(self) -> float:
        # Returns interior width
        return 0.0


class Car(Vehicle):
    
    @property
    def interior_width(self) -> float:
        return self.cabin_width
    
    @property
    def cabin_width(self) -> float:
        return 1.23


class RacingCar(Vehicle):
    
    @property
    def interior_width(self) -> float:
        return self.cookpit_width
    
    @property
    def cookpit_width(self) -> float:
        return 1.23
```

With `interior_width` we have a more generic attribute that can be replaced with `cabin_width` on
`Car` class, and with `cookpit_width` for `RacingCar`.

Now if we want to use both classes for returning the interior width, we could have:

```python
class VehicleUtils:
    
    def print_interior_width(self):
        vehicle_1 = Car()
        vehicle_2 = Car()
        vehicle_3 = RacingCar()
    
        vehicles = [vehicle_1, vehicle_2, vehicle_3]
    
        for vehicle in vehicles:
            print(vehicle.interior_width)
```

This will work for any class that inherit from `Vehicle`, either a `Car`, a `RacingCar`, or any
vehicle.

## Tell, Don't Ask

Imagine now that we have a `Product` class that describes a product, and a `InHouseProduct` that
describes a product made by the store. E.g.: if we think in Amazon Ecommerce, we have products
from numerous sellers (`Product`), and we have products made by Amazon ifself, like the Echo Dots
(`InHouseProduct`).

If we have a funcion that describes a base discount of 20% for all products and 1.5x for in house
products, we would have this code:

```python
class Product:

    def __init__(self) -> None:
        self._discount = 20

    def get_discount(self) -> float:
        return self._discount


class InHouseProduct(Product):

    def apply_extra_discount(self) -> None:
        self._discount = self._discount * 1.5
```

And like in the example before, if we had a utils class that prints out all our discounts, we would
have this:

```python
class ProductUtils:

    def print_product_discount(self):
        product_1 = Product()
        product_2 = Product()
        product_3 = InHouseProduct()

        products = [product_1, product_2, product_3]

        for product in products:
            if isinstance(product, InHouseProduct):
                product.apply_extra_discount()

            print(product.get_discount())
```

This is a bad design, because we are not dealing with all products as equal, we are needing to
perform an additional code for `InHouseProduct`.

Following the LSP, we can rewrite the `InHouseProduct` class to this:

```python
class InHouseProduct(Product):

    def get_discount(self) -> float:
        self._apply_extra_discount()
        return self._discount

    def _apply_extra_discount(self) -> None:
        self._discount = self._discount * 1.5
```

This way we are overriding the parent method `get_discount` from `Product` class, we are not asking
if `product` is a `InHouseProduct`, we are just telling the code to `get_discount`, applying the
`Tell, Don't Ask` concept.

So now our `ProductUtils` can be rewritten to:

```python
class ProductUtils:

    def print_product_discount(self):
        product_1 = Product()
        product_2 = Product()
        product_3 = InHouseProduct()

        products = [product_1, product_2, product_3]

        for product in products:
            print(product.get_discount())
```
