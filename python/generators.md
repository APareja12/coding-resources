# Python Generators

## Overview
Generators are special functions in Python that return an iterator which yields values one at a time using the `yield` keyword instead of `return`. Unlike regular functions that compute all values at once and return them in a list, generators produce values on-the-fly (lazy evaluation), making them incredibly memory-efficient for working with large datasets or infinite sequences. Once a value is yielded, the generator pauses and remembers its state until the next value is requested.

## Key Concepts
- **yield keyword** - Pauses function and returns a value, resuming from that point next time
- **Lazy evaluation** - Values are produced only when needed, not all at once
- **Memory efficient** - Only one value in memory at a time, not entire sequence
- **Iterator protocol** - Generators automatically implement `__iter__()` and `__next__()`
- **State preservation** - Generator remembers its state between yields
- **Generator expressions** - Concise syntax similar to list comprehensions

## Code Example
```python
# Basic generator function
def count_up_to(n):
    count = 1
    while count <= n:
        yield count
        count += 1

# Using the generator
counter = count_up_to(5)
print(next(counter))  # 1
print(next(counter))  # 2
print(next(counter))  # 3

# Or iterate through all values
for num in count_up_to(5):
    print(num)  # Prints 1, 2, 3, 4, 5


# Generator vs regular function comparison
# Regular function - loads everything into memory
def get_squares_list(n):
    result = []
    for i in range(n):
        result.append(i ** 2)
    return result

squares = get_squares_list(1000000)  # Uses lots of memory!

# Generator - produces values one at a time
def get_squares_generator(n):
    for i in range(n):
        yield i ** 2

squares_gen = get_squares_generator(1000000)  # Uses minimal memory!


# Practical example: Reading large files
def read_large_file(file_path):
    """Read file line by line without loading entire file into memory"""
    with open(file_path, 'r') as file:
        for line in file:
            yield line.strip()

# Usage
for line in read_large_file('huge_log_file.txt'):
    if 'ERROR' in line:
        print(line)


# Generator with multiple yields
def fibonacci(limit):
    """Generate Fibonacci numbers up to limit"""
    a, b = 0, 1
    while a < limit:
        yield a
        a, b = b, a + b

for num in fibonacci(100):
    print(num)  # 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89


# Generator expressions (like list comprehensions)
# List comprehension - creates entire list in memory
squares_list = [x ** 2 for x in range(1000000)]

# Generator expression - creates generator object
squares_gen = (x ** 2 for x in range(1000000))

# Get first 5 squares efficiently
first_five = [next(squares_gen) for _ in range(5)]
print(first_five)  # [0, 1, 4, 9, 16]


# Practical example: Data processing pipeline
def read_csv_data(filename):
    """Yield each row from CSV"""
    with open(filename) as f:
        next(f)  # Skip header
        for line in f:
            yield line.strip().split(',')

def filter_valid_rows(rows):
    """Yield only rows with valid data"""
    for row in rows:
        if len(row) == 3 and row[2].isdigit():
            yield row

def transform_data(rows):
    """Convert data types"""
    for name, email, age in rows:
        yield {
            'name': name.strip(),
            'email': email.strip().lower(),
            'age': int(age)
        }

# Chain generators together
data = read_csv_data('users.csv')
valid = filter_valid_rows(data)
transformed = transform_data(valid)

# Process one record at a time
for user in transformed:
    print(user)


# Infinite generator
def infinite_sequence():
    """Generate infinite sequence of numbers"""
    num = 0
    while True:
        yield num
        num += 1

# Safe to create - doesn't actually generate infinite numbers yet!
gen = infinite_sequence()
print(next(gen))  # 0
print(next(gen))  # 1
# Can keep calling next() forever


# Generator with send() - two-way communication
def running_average():
    """Calculate running average of sent values"""
    total = 0
    count = 0
    average = None
    while True:
        value = yield average
        total += value
        count += 1
        average = total / count

avg = running_average()
next(avg)  # Prime the generator
print(avg.send(10))  # 10.0
print(avg.send(20))  # 15.0
print(avg.send(30))  # 20.0


# Memory comparison example
import sys

# List - high memory usage
list_comp = [x for x in range(1000000)]
print(f"List size: {sys.getsizeof(list_comp)} bytes")  # ~8 MB

# Generator - minimal memory
gen_exp = (x for x in range(1000000))
print(f"Generator size: {sys.getsizeof(gen_exp)} bytes")  # ~200 bytes
```

## Common Use Cases
- **Large file processing** - Read huge files line by line without memory issues
- **Data pipelines** - Chain operations without creating intermediate lists
- **Infinite sequences** - Generate unlimited data streams (IDs, timestamps)
- **Database queries** - Process rows one at a time instead of loading all results
- **Web scraping** - Yield scraped items as they're found
- **Stream processing** - Handle real-time data feeds efficiently

## Additional Resources
- [Python Documentation: Generators](https://docs.python.org/3/howto/functional.html#generators)
- [Real Python: Introduction to Generators](https://realpython.com/introduction-to-python-generators/)
- [PEP 255: Simple Generators](https://www.python.org/dev/peps/pep-0255/)
- [Understanding Generator Expressions](https://realpython.com/list-comprehension-python/#using-generator-expressions)