# 04 Interface Segregation Principle (ISP)

- No clients should be forced to depend on methods it does not use.

## Example

Think a scenario that we have a office that has many printers, we want to code these printers
classes. Using interfaces, we can define as the following:

```python
class MultiFunctionInterface:

    @abstract_method
    def print(self) -> None:
        pass

    @abstract_method
    def get_print_spool_details(self) -> None:
        pass

    @abstract_method
    def scan(self) -> None:
        pass

    @abstract_method
    def scan_proto(self) -> None:
        pass

    @abstract_method
    def fax(self) -> None:
        pass

    @abstract_method
    def internet_fax(self) -> None:
        pass
```

If we have a printer model `XeroxWorkCentre`, which is a full multi function with print, scan and
fax, we should implement all methods from the interface.

```python
class XeroxWorkCentre(MultiFunctionInterface):

    def print(self) -> None:
        # Assume real printing code
        pass

    def get_print_spool_details(self) -> None:
        # Assume real code that gets spool details
        pass

    def scan(self) -> None:
        # Assume real code for scanning
        pass

    def scan_proto(self) -> None:
        # Assume real code for photo scans
        pass

    def fax(self) -> None:
        # Assume real code for fax
        pass

    def internet_fax(self) -> None:
        # Assume real code for internet fax
        pass
```

In the other office's room, we have another printer model called `HPPrinterNScanner`, this one only
prints and scans, if does not do fax. That way, we would have to defined the fax methods, but have
it implementation empty.

```python
class HPPrinterNScanner(MultiFunctionInterface):

    def print(self) -> None:
        # Assume real printing code
        pass

    def get_print_spool_details(self) -> None:
        # Assume real code that gets spool details
        pass

    def scan(self) -> None:
        # Assume real code for scanning
        pass

    def scan_proto(self) -> None:
        # Assume real code for photo scans
        pass

    def fax(self) -> None:
        pass

    def internet_fax(self) -> None:
        pass
```

Walking through the office, there is another printer model, `CannonPrinter`, that one is much
simpler, it only prints, it does not scan or fax.

```python
class CannonPrinter(MultiFunctionInterface):

    def print(self) -> None:
        # Assume real printing code
        pass

    def get_print_spool_details(self) -> None:
        # Assume real code that gets spool details
        pass

    def scan(self) -> None:
        pass

    def scan_proto(self) -> None:
        pass

    def fax(self) -> None:
        pass

    def internet_fax(self) -> None:
        pass
```

This is a very poor design. We have two classes that have many methods without any implementation,
e.g. if a developer tries to use the function `scan` of `CannonPrinter`, it will do nothing and it's
probably going to break the code, because the system is expecting the `CannonPrinter` to `scan`.

Following the ISP to make this code better, we can split the `MultiFunctionInterface` into smaller
interfaces.

```python
class PrintInterface:

    @abstract_method
    def print(self) -> None:
        pass

    @abstract_method
    def get_print_spool_details(self) -> None:
        pass


class ScanInterface:

    @abstract_method
    def scan(self) -> None:
        pass

    @abstract_method
    def scan_proto(self) -> None:
        pass


class FaxInterface:

    @abstract_method
    def fax(self) -> None:
        pass

    @abstract_method
    def internet_fax(self) -> None:
        pass
```

And now our printer classes can implement the interfaces that makes sense for each model and only
implements the methods they need.

```python
class XeroxWorkCentre(PrintInterface, ScanInterface, FaxInterface):

    def print(self) -> None:
        # Assume real printing code
        pass

    def get_print_spool_details(self) -> None:
        # Assume real code that gets spool details
        pass

    def scan(self) -> None:
        # Assume real code for scanning
        pass

    def scan_proto(self) -> None:
        # Assume real code for photo scans
        pass

    def fax(self) -> None:
        # Assume real code for fax
        pass

    def internet_fax(self) -> None:
        # Assume real code for internet fax
        pass


class HPPrinterNScanner(PrintInterface, ScanInterface):

    def print(self) -> None:
        # Assume real printing code
        pass

    def get_print_spool_details(self) -> None:
        # Assume real code that gets spool details
        pass

    def scan(self) -> None:
        # Assume real code for scanning
        pass

    def scan_proto(self) -> None:
        # Assume real code for photo scans
        pass


class CannonPrinter(PrintInterface):

    def print(self) -> None:
        # Assume real printing code
        pass

    def get_print_spool_details(self) -> None:
        # Assume real code that gets spool details
        pass
```

Now we do not have any blank implementation on any method of our classes. Also if we do have a
method that all three interfaces need, we can create a parent class to abstract this.

## Tecniques to Identify Violations of ISP

1. Fat interfaces.
2. Interfaces with low cohesion.
3. Empty methods implementation.
