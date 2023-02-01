# 01 Single Responsability Principle (SRP)

- Every software component should have one and only one responsability.
- Component can be a class, method or a module.
- Aim for High **Cohesion**.
- Aim for Loose **Couping**.

## Key Concepts: Cohesion

- Cohesion is the degree to which the various parts of a software component are related.
- Higher cohesion helps attain better adherence to the Single Responsability Principle.

### Example

#### Before

```python
class Square:
    def __init__(self):
        self.side = 5

    def calculate_area(self):
        return self.side * self.side

    def calculate_perimeter(self):
        return self.side * 4

    def draw(self):
        if high_resolution_monitor:
            # Render a high resolution image of a square
            pass
        else:
            # Render a normal image of a squad
            pass

    def rotate(self, degree):
        # Rotate the image of the square clockwise to the required degree and re-render
        pass
```

#### After

```python
# Single Responsability: Measurements of squares
class Square:
    def __init__(self):
        self.side = 5

    def calculate_area(self):
        return self.side * self.side

    def calculate_perimeter(self):
        return self.side * 4


# Single Responsability: Rendering images of square
class SquareUI:
    def draw(self):
        if high_resolution_monitor:
            # Render a high resolution image of a square
            pass
        else:
            # Render a normal image of a squad
            pass

    def rotate(self, degree):
        # Rotate the image of the square clockwise to the required degree and re-render
        pass
```

## Key Concepts: Coupling

- Coupling is defined as the level of inter dependecy between various sofware components.
- Loose coupling helps attain better adherence to the Single Responsability Principle.

### Example

#### Before

```python
class Student:
    def __init__(self):
        self.id = None
        self.name = None
        self.age = None

    def save(self):
        sql_query = f'INSERT INTO student (name, age) VALUES ({self.name}, {self.age})'

        try:
            db.session.begin()
            connection = db.session.connection()
            connection.execute(sql_query, params)
            db.session.commit()

        except Exception as error:
            db.session.rollback()
            logging.error('Failed to save student on database')
            raise error

    def get_year_of_birth(self):
        today = datetime.date.today()
        current_year = today.year
        return current_year - self.age

    ...
```

#### After

```python
# Single Responsability: Handle core student profile data
class Student:
    def __init__(self):
        self.id = None
        self.name = None
        self.age = None

    def save(self):
        StudentRepository().save(student=self)

    def get_year_of_birth(self):
        today = datetime.date.today()
        current_year = today.year
        return current_year - self.age

    ...


# Single Responsability: Handle database operations
class StudentRepository:

    def save(self, student):
        sql_query = f'INSERT INTO student (name, age) VALUES ({student.name}, {student.age})'

        try:
            db.session.begin()
            connection = db.session.connection()
            connection.execute(sql_query, params)
            db.session.commit()

        except Exception as error:
            db.session.rollback()
            logging.error('Failed to save student on database')
            raise error
```

## Reason to Change

Another way of visualize the **Single Responsability Principle**, is to think that "Every software
component should have one and only one <s>responsability</s> reason to change".

Looking on the example above, we can see that the first `Student` class has 2 reasons to change:

1. Changes to student profile (id, name, ...).
2. Changes in the database backend, such as platform.

Rewriting the `Student` class aiming to loose couping, we break it into two new classes, each one
having only one reason to change:

- `Student` class: changes to student profile (id, name, ...).
- `StudentRepository` class: changes in the database backend, such as platform.

This point of view reinforce the SRP into the software.
