### 1) What are Python lists and how are they different from tuples?

**Python Lists**:
- **Definition**: A list is a mutable, ordered collection of items. Lists can contain items of different data types.
- **Syntax**: Lists are defined using square brackets `[ ]`.

  ```python
  my_list = [1, 2, 3, "four", 5.0]
  ```
  
- **Mutability**: Lists are mutable, meaning their elements can be changed after the list is created.
  ```python
  my_list[0] = 10  # Changes the first element to 10
  ```
- **Methods**: Lists have various built-in methods like `append()`, `remove()`, `pop()`, etc.

**Python Tuples**:
- **Definition**: A tuple is an immutable, ordered collection of items. Like lists, tuples can contain items of different data types.
- **Syntax**: Tuples are defined using parentheses `( )`.
  ```python
  my_tuple = (1, 2, 3, "four", 5.0)
  ```
- **Immutability**: Tuples are immutable, meaning their elements cannot be changed after the tuple is created.
  ```python
  # Attempting to change a tuple element will raise an error
  my_tuple[0] = 10  # Raises TypeError: 'tuple' object does not support item assignment
  ```
- **Uses**: Tuples are often used for fixed collections of items, like coordinates or function returns.

### 2) How do you create a dictionary in Python and access its values?

**Creating a Dictionary**:
- **Syntax**: Dictionaries are created using curly braces `{ }` with key-value pairs.
  ```python
  my_dict = {"name": "John", "age": 30, "city": "New York"}
  ```

**Accessing Values**:
- **By Key**: Values are accessed using their corresponding keys.
  ```python
  name = my_dict["name"]  # Output: "John"
  age = my_dict.get("age")  # Output: 30
  ```
- **Using `get()` Method**: The `get()` method is useful for accessing values without raising an error if the key does not exist.
  ```python
  country = my_dict.get("country", "USA")  # Output: "USA" (default value)
  ```

### 3) Explain list comprehension and provide an example.

**List Comprehension**:
- **Definition**: List comprehension is a concise way to create lists. It consists of brackets containing an expression followed by a `for` clause, and optionally, one or more `if` clauses.
- **Syntax**:
  ```python
  [expression for item in iterable if condition]
  ```
- **Example**: Creating a list of squares of even numbers from 0 to 9.
  ```python
  squares = [x**2 for x in range(10) if x % 2 == 0]
  print(squares)  # Output: [0, 4, 16, 36, 64]
  ```

### 4) How can you read a CSV file in Python using pandas?

**Reading a CSV File**:
- **Using `pandas.read_csv()`**: The `read_csv()` function from the `pandas` library is used to read CSV files.
  ```python
  import pandas as pd

  # Read the CSV file into a DataFrame
  df = pd.read_csv("file_path.csv")
  ```
- **Specifying Additional Parameters**: You can specify additional parameters such as `delimiter`, `header`, `names`, etc.
  ```python
  df = pd.read_csv("file_path.csv", delimiter=",", header=0)
  ```

### 5) What is the difference between loc and iloc in pandas?

**`loc` and `iloc`**:
- **`loc`**:
  - **Definition**: `loc` is label-based, meaning that you have to specify rows and columns based on their labels.
  - **Usage**: It is used to access a group of rows and columns by labels or a boolean array.
  ```python
  # Access row by index label and column by column label
  row = df.loc[1, 'column_name']
  # Access rows by boolean array
  rows = df.loc[df['column_name'] > 10]
  ```

- **`iloc`**:
  - **Definition**: `iloc` is integer-location-based, meaning that you have to specify rows and columns by their integer position.
  - **Usage**: It is used to access a group of rows and columns by integer positions (indices).
  ```python
  # Access row by integer index and column by integer index
  row = df.iloc[0, 1]
  # Access multiple rows and columns by integer indices
  rows = df.iloc[0:5, 1:3]
  ```

By understanding these Python and pandas concepts, you can effectively manage and analyze data, leveraging the powerful functionalities provided by the language and the library.

### 6) How do you handle missing data in a pandas DataFrame?

Handling missing data in pandas can be done in several ways:

1. **Identifying Missing Data**:
   ```python
   import pandas as pd

   # Create a DataFrame with missing values
   df = pd.DataFrame({
       'A': [1, 2, None, 4],
       'B': [None, 2, 3, 4],
       'C': [1, None, None, 4]
   })

   # Check for missing values
   print(df.isnull())
   print(df.isnull().sum())
   ```

2. **Removing Missing Data**:
   ```python
   # Drop rows with any missing values
   df_cleaned = df.dropna()

   # Drop columns with any missing values
   df_cleaned = df.dropna(axis=1)

   # Drop rows with missing values in a specific column
   df_cleaned = df.dropna(subset=['A'])
   ```

3. **Filling Missing Data**:
   ```python
   # Fill missing values with a specific value
   df_filled = df.fillna(0)

   # Fill missing values with the mean of the column
   df_filled = df.fillna(df.mean())

   # Forward fill (fill with the previous value)
   df_filled = df.ffill()

   # Backward fill (fill with the next value)
   df_filled = df.bfill()
   ```

### 7) Explain the use of the apply() function in pandas.

The `apply()` function in pandas is used to apply a function along an axis of the DataFrame (rows or columns).

1. **Applying a function to each column**:
   ```python
   import pandas as pd

   df = pd.DataFrame({
       'A': [1, 2, 3, 4],
       'B': [10, 20, 30, 40]
   })

   # Apply a function to each column
   df_applied = df.apply(lambda x: x * 2)
   ```

2. **Applying a function to each row**:
   ```python
   # Apply a function to each row
   df_applied = df.apply(lambda x: x.sum(), axis=1)
   ```

3. **Using a predefined function**:
   ```python
   def square(x):
       return x * x

   df_applied = df.apply(square)
   ```

### 8) How can you merge/join two DataFrames in pandas?

Merging or joining DataFrames in pandas can be done using the `merge()` function or the `join()` method.

1. **Using `merge()`**:
   ```python
   import pandas as pd

   df1 = pd.DataFrame({
       'key': ['A', 'B', 'C', 'D'],
       'value1': [1, 2, 3, 4]
   })

   df2 = pd.DataFrame({
       'key': ['A', 'B', 'E', 'F'],
       'value2': [5, 6, 7, 8]
   })

   # Inner join
   df_merged = pd.merge(df1, df2, on='key', how='inner')

   # Outer join
   df_merged = pd.merge(df1, df2, on='key', how='outer')

   # Left join
   df_merged = pd.merge(df1, df2, on='key', how='left')

   # Right join
   df_merged = pd.merge(df1, df2, on='key', how='right')
   ```

2. **Using `join()`**:
   ```python
   df1 = df1.set_index('key')
   df2 = df2.set_index('key')

   # Join DataFrames on their index
   df_joined = df1.join(df2, how='inner')
   ```

### 9) Describe how to group data in pandas and perform aggregation.

Grouping data in pandas is done using the `groupby()` method followed by an aggregation function.

1. **Grouping and Aggregating**:
   ```python
   import pandas as pd

   df = pd.DataFrame({
       'category': ['A', 'A', 'B', 'B'],
       'value': [10, 20, 30, 40]
   })

   # Group by 'category' and calculate the sum of 'value'
   grouped_df = df.groupby('category').sum()

   # Group by 'category' and calculate multiple aggregations
   grouped_df = df.groupby('category').agg({'value': ['sum', 'mean', 'count']})
   ```

2. **Grouping by Multiple Columns**:
   ```python
   df = pd.DataFrame({
       'category1': ['A', 'A', 'B', 'B'],
       'category2': ['X', 'Y', 'X', 'Y'],
       'value': [10, 20, 30, 40]
   })

   # Group by 'category1' and 'category2'
   grouped_df = df.groupby(['category1', 'category2']).sum()
   ```

### 10) What are NumPy arrays and how do they differ from Python lists?

**NumPy Arrays**:
- **Definition**: NumPy arrays are multi-dimensional, fixed-size arrays that contain elements of the same data type. They are provided by the NumPy library, which is used for numerical computations.
- **Syntax**: NumPy arrays are created using the `numpy.array()` function.
  ```python
  import numpy as np

  np_array = np.array([1, 2, 3, 4, 5])
  ```

**Differences from Python Lists**:
1. **Data Type**: All elements in a NumPy array must be of the same data type, while a Python list can contain elements of different data types.
   ```python
   # NumPy array
   np_array = np.array([1, 2, 3])
   print(np_array.dtype)  # Output: int32 (or int64)

   # Python list
   py_list = [1, 'two', 3.0]
   ```

2. **Performance**: NumPy arrays are more memory-efficient and faster for numerical computations compared to Python lists because they are implemented in C and use contiguous blocks of memory.
   ```python
   import time

   py_list = list(range(1000000))
   np_array = np.array(py_list)

   start_time = time.time()
   py_list_sum = sum(py_list)
   print("Python list sum time:", time.time() - start_time)

   start_time = time.time()
   np_array_sum = np.sum(np_array)
   print("NumPy array sum time:", time.time() - start_time)
   ```

3. **Operations**: NumPy arrays support element-wise operations and broadcasting, which are not directly available for Python lists.
   ```python
   np_array = np.array([1, 2, 3])
   result = np_array * 2  # Element-wise multiplication

   py_list = [1, 2, 3]
   result = [x * 2 for x in py_list]  # List comprehension for element-wise multiplication
   ```

By understanding these pandas and NumPy concepts, you can effectively manipulate and analyze data, leveraging the powerful functionalities provided by these libraries.

### 11) How do you perform element-wise operations on NumPy arrays?

Element-wise operations on NumPy arrays allow you to perform arithmetic and other operations on each element of the array. Here are some examples:

1. **Arithmetic Operations**:
   ```python
   import numpy as np

   a = np.array([1, 2, 3])
   b = np.array([4, 5, 6])

   # Element-wise addition
   c = a + b  # Output: array([5, 7, 9])

   # Element-wise subtraction
   d = a - b  # Output: array([-3, -3, -3])

   # Element-wise multiplication
   e = a * b  # Output: array([4, 10, 18])

   # Element-wise division
   f = a / b  # Output: array([0.25, 0.4, 0.5])
   ```

2. **Mathematical Functions**:
   ```python
   # Element-wise square
   g = np.square(a)  # Output: array([1, 4, 9])

   # Element-wise square root
   h = np.sqrt(a)  # Output: array([1.0, 1.41421356, 1.73205081])

   # Element-wise sine
   i = np.sin(a)  # Output: array([0.84147098, 0.90929743, 0.14112001])
   ```

3. **Comparison Operations**:
   ```python
   # Element-wise comparison
   j = a > 2  # Output: array([False, False, True])

   # Element-wise equality
   k = a == b  # Output: array([False, False, False])
   ```

### 12) What is the use of the Matplotlib library in Python? Provide an example of a simple plot.

Matplotlib is a widely used library in Python for creating static, animated, and interactive visualizations. It provides a wide range of plotting functions to visualize data.

**Example of a Simple Plot**:
```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 3, 5, 7, 11]

# Create a simple line plot
plt.plot(x, y, marker='o')

# Add titles and labels
plt.title("Simple Line Plot")
plt.xlabel("X-axis")
plt.ylabel("Y-axis")

# Show the plot
plt.show()
```

### 13) How do you create subplots in Matplotlib?

Subplots allow you to create multiple plots in a single figure. You can create subplots using the `plt.subplot()` function or the `plt.subplots()` method.

1. **Using `plt.subplot()`**:
   ```python
   import matplotlib.pyplot as plt

   # Create a 2x1 grid of subplots
   plt.subplot(2, 1, 1)
   plt.plot([1, 2, 3], [4, 5, 6])
   plt.title('Subplot 1')

   plt.subplot(2, 1, 2)
   plt.plot([1, 2, 3], [6, 5, 4])
   plt.title('Subplot 2')

   plt.tight_layout()  # Adjust layout to prevent overlap
   plt.show()
   ```

2. **Using `plt.subplots()`**:
   ```python
   import matplotlib.pyplot as plt

   # Create a 2x2 grid of subplots
   fig, axs = plt.subplots(2, 2)

   # First subplot
   axs[0, 0].plot([1, 2, 3], [4, 5, 6])
   axs[0, 0].set_title('Subplot 1')

   # Second subplot
   axs[0, 1].plot([1, 2, 3], [6, 5, 4])
   axs[0, 1].set_title('Subplot 2')

   # Third subplot
   axs[1, 0].plot([1, 2, 3], [7, 8, 9])
   axs[1, 0].set_title('Subplot 3')

   # Fourth subplot
   axs[1, 1].plot([1, 2, 3], [9, 8, 7])
   axs[1, 1].set_title('Subplot 4')

   plt.tight_layout()  # Adjust layout to prevent overlap
   plt.show()
   ```

### 14) Explain the use of the Seaborn library and provide an example of a categorical plot.

Seaborn is a Python data visualization library based on Matplotlib that provides a high-level interface for drawing attractive and informative statistical graphics.

**Example of a Categorical Plot**:
```python
import seaborn as sns
import matplotlib.pyplot as plt

# Load a dataset
tips = sns.load_dataset("tips")

# Create a categorical plot (box plot)
sns.boxplot(x="day", y="total_bill", data=tips)

# Add titles and labels
plt.title("Box Plot of Total Bill by Day")
plt.xlabel("Day")
plt.ylabel("Total Bill")

# Show the plot
plt.show()
```

### 15) What is a lambda function in Python and how is it used?

A lambda function in Python is a small anonymous function defined using the `lambda` keyword. It can have any number of arguments but only one expression. Lambda functions are often used for short, simple operations.

**Syntax**:
```python
lambda arguments: expression
```

**Example**:
1. **Simple Lambda Function**:
   ```python
   add = lambda x, y: x + y
   print(add(5, 3))  # Output: 8
   ```

2. **Lambda Function in `map()`**:
   ```python
   numbers = [1, 2, 3, 4, 5]
   squares = list(map(lambda x: x * x, numbers))
   print(squares)  # Output: [1, 4, 9, 16, 25]
   ```

3. **Lambda Function in `filter()`**:
   ```python
   numbers = [1, 2, 3, 4, 5]
   even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
   print(even_numbers)  # Output: [2, 4]
   ```

Lambda functions provide a concise way to write small functions and are often used in conjunction with functions like `map()`, `filter()`, and `reduce()`.

### 16) Describe how to filter a DataFrame based on a condition.

Filtering a DataFrame based on a condition is straightforward in pandas. You can use boolean indexing to select rows that meet specific criteria.

**Example**:
```python
import pandas as pd

# Sample DataFrame
data = {'name': ['Alice', 'Bob', 'Charlie', 'David'],
        'age': [24, 27, 22, 32],
        'score': [85, 78, 90, 88]}

df = pd.DataFrame(data)

# Filter rows where age is greater than 25
filtered_df = df[df['age'] > 25]

print(filtered_df)
```

**Output**:
```
   name  age  score
1   Bob   27     78
3 David   32     88
```

### 17) How do you use the datetime module to manipulate dates and times in Python?

The `datetime` module in Python provides classes for manipulating dates and times. 

**Common Operations**:

1. **Current Date and Time**:
   ```python
   from datetime import datetime

   now = datetime.now()
   print(now)  # Output: current date and time
   ```

2. **Create a Specific Date or Time**:
   ```python
   from datetime import datetime

   dt = datetime(2024, 7, 10, 12, 30, 45)
   print(dt)  # Output: 2024-07-10 12:30:45
   ```

3. **Date Arithmetic**:
   ```python
   from datetime import datetime, timedelta

   dt = datetime(2024, 7, 10)
   future_date = dt + timedelta(days=10)
   print(future_date)  # Output: 2024-07-20
   ```

4. **Formatting Dates**:
   ```python
   from datetime import datetime

   dt = datetime(2024, 7, 10, 12, 30, 45)
   formatted_date = dt.strftime("%Y-%m-%d %H:%M:%S")
   print(formatted_date)  # Output: 2024-07-10 12:30:45
   ```

5. **Parsing Dates**:
   ```python
   from datetime import datetime

   date_string = "2024-07-10 12:30:45"
   dt = datetime.strptime(date_string, "%Y-%m-%d %H:%M:%S")
   print(dt)  # Output: 2024-07-10 12:30:45
   ```

### 18) Explain the difference between a shallow copy and a deep copy in Python.

In Python, a shallow copy creates a new object, but does not create copies of nested objects. Instead, it inserts references into the new object to the original objects. A deep copy creates a new object and recursively copies all objects found in the original.

1. **Shallow Copy**:
   ```python
   import copy

   original = [1, [2, 3], 4]
   shallow_copy = copy.copy(original)
   
   shallow_copy[1][0] = 99
   print(original)      # Output: [1, [99, 3], 4]
   print(shallow_copy)  # Output: [1, [99, 3], 4]
   ```

2. **Deep Copy**:
   ```python
   import copy

   original = [1, [2, 3], 4]
   deep_copy = copy.deepcopy(original)
   
   deep_copy[1][0] = 99
   print(original)     # Output: [1, [2, 3], 4]
   print(deep_copy)    # Output: [1, [99, 3], 4]
   ```

### 19) How can you perform data normalization or standardization in Python?

Data normalization (scaling the data to a specific range) and standardization (scaling data to have a mean of 0 and a standard deviation of 1) can be done using libraries like `scikit-learn`.

1. **Normalization**:
   ```python
   from sklearn.preprocessing import MinMaxScaler
   import numpy as np

   data = np.array([[1, 2], [2, 3], [4, 5]])

   scaler = MinMaxScaler()
   normalized_data = scaler.fit_transform(data)
   
   print(normalized_data)
   # Output: 
   # [[0.   0.  ]
   #  [0.333 0.333]
   #  [1.   1.  ]]
   ```

2. **Standardization**:
   ```python
   from sklearn.preprocessing import StandardScaler
   import numpy as np

   data = np.array([[1, 2], [2, 3], [4, 5]])

   scaler = StandardScaler()
   standardized_data = scaler.fit_transform(data)
   
   print(standardized_data)
   # Output:
   # [[-1.069  -1.069 ]
   #  [-0.267  -0.267]
   #  [ 1.336   1.336]]
   ```

### 20) Describe how to use regular expressions in Python for data cleaning.

Regular expressions (regex) in Python are handled by the `re` module and are used for matching and manipulating strings based on specific patterns.

**Common Operations**:

1. **Matching**:
   ```python
   import re

   pattern = r"\d+"  # Matches one or more digits
   text = "There are 123 apples"

   match = re.search(pattern, text)
   print(match.group())  # Output: 123
   ```

2. **Finding All Matches**:
   ```python
   matches = re.findall(pattern, text)
   print(matches)  # Output: ['123']
   ```

3. **Replacing**:
   ```python
   new_text = re.sub(pattern, "many", text)
   print(new_text)  # Output: There are many apples
   ```

4. **Splitting**:
   ```python
   split_text = re.split(r"\s", text)  # Splits by whitespace
   print(split_text)  # Output: ['There', 'are', '123', 'apples']
   ```

5. **Cleaning Data Example**:
   ```python
   import pandas as pd

   data = {'text': ['John_123', 'Alice_456', 'Bob_789']}
   df = pd.DataFrame(data)

   # Remove numbers from text
   df['clean_text'] = df['text'].apply(lambda x: re.sub(r'\d+', '', x))
   
   print(df)
   # Output:
   #       text clean_text
   # 0  John_123       John_
   # 1  Alice_456      Alice_
   # 2    Bob_789       Bob_
   ```

These operations help in cleaning, transforming, and extracting valuable information from textual data.
