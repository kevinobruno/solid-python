# 02 Open-Closed Principle (OCP)

- Software components should be closed for modification, but open for extension.
- **Closed For Modification**: new features getting added to the software component should NOT have
to modify existing code.
- **Open For Extension**: A software component should be extendable to add a new feature or to add a
new behavior to it.
- Key Takeaways:
    - Ease of adding new features.
    - Leads to minimal cost of developing and testing software.
    - OCP often requires decouping, which, in turn, automatically follows the
    [Single Responsability Principle][01-single-responsability-principle].

## Example

Imagine we work on an **Health Insurance** company that provides discount to loyal customers, we
would have some of it to provide this discount:

```python
class HealthInsuranceCustomerProfile:

    @property
    def is_loyal(self) -> bool:
        return True  # or False, depending on where we would save this info.


class InsurancePremiumDiscountCalculator:

    def calculate_premium_discount_percent(self, customer: HealthInsuranceCustomerProfile) -> int:
        if customer.is_loyal:
            return 20

        return 0
```

This works, we have the class `InsurancePremiumDiscountCalculator` able to calculate the discount
basead on whether or not the customer is loyal.

Imagine now the company does not provide only **Health Insurance**, but also **Vehicle Insurance**,
how would we maintain the discount calculator to those customers wanting an vehicle insurance?

We could build another method on `InsurancePremiumDiscountCalculator` that receives a
`VehicleInsuranceCustomerProfile` as parameter and calculate the discount as we had before, such as:

```python
class InsurancePremiumDiscountCalculator:

    def calculate_premium_discount_percent_for_health(self, customer: HealthInsuranceCustomerProfile) -> int:
        if customer.is_loyal:
            return 20

        return 0

    def calculate_premium_discount_percent_for_vehicle(self, customer: VehicleInsuranceCustomerProfile) -> int:
        if customer.is_loyal:
            return 20

        return 0
```

This works but is bad, we had to create another method that calculates the discount and as those
methods starts to gain complexity, we will need to make multiple changes to satisfy those needs,
besides the fact that if the company offers new insurances, such as an **Home Insurance** for
example, we would need to create an third method for this discount.

Following OCP, we could create an abstract class, Java call these Interface, and each language
call this something different, but the essence is to create a class that encapsulate those needs and
the other classes just implements what those class demands.

Rewriting the codes above creating an abstract class: 

```python
class CustomerProfile:

    @property
    def is_loyal(self) -> bool:
        raise NotImplementedError()


class HealthInsuranceCustomerProfile(CustomerProfile):

    @property
    def is_loyal(self) -> bool:
        return True  # or False, depending on where we would save this info.


class VehicleInsuranceCustomerProfile(CustomerProfile):

    @property
    def is_loyal(self) -> bool:
        return True  # or False, depending on where we would save this info.
```

This way the have a `CustomerProfile` class that demands the property `is_loyal` is defined, and
we have another two classes that inherits this class and implements the `is_loyal` property as
demanded.

Now the `InsurancePremiumDiscountCalculator` class can receive only a `CustomerProfile` to calculate
the discount:

```python
class InsurancePremiumDiscountCalculator:

    def calculate_premium_discount_percent(self, customer: CustomerProfile) -> int:
        if customer.is_loyal:
            return 20

        return 0
```

This way we can have as many `CustomerProfile` classes as we want, the class that calculates the
discount will never have to change its code or behavior because of a new `CustomerProfile`, that
means this discount feature is **closed for modification** but **open for extension**.

[01-single-responsability-principle]: 01-single-responsability-principle.md
