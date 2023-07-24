---
layout: single
title:  "Input validation and type conversion with Pydantic in python"
date:   2023-07-16 17:45:00 +1000
tags: pydantic data validation 
excerpt: "In this blog post we will explore how to use the Pydantic python package to streamline your code and enhance data validation."
---

![Pydantic splash image]({{ site.url }}{{ site.baseurl }}/assets/images/pydantic.png)
{: .left}



## Introduction

 Pydantic is a library that provides runtime checking and validation of Python objects  attributes using python s type hinting (python 3.7 and above). It offers an intuitive way to define data schemas as classes allowing you to validate inputs against those schemas effortlessly. li
 
 There are many use cases for Pydantic which include but are not limited to:

- **Data Validation**: 
Pydantic allows you to define data models using Python annotations which automatically validates the input data against the defined model. This ensures that your data is always in the expected format reducing the chances of errors or unexpected behavior.
 
- **Automatic Documentation**:
Defining  data models using Pydantic allows you to generate detailed documentation automatically -including information about field types default values and constraints making it easier for other developers to understand and use your code.
 
- **Serialization and Deserialization**:
Pydantic simplifies the process of serializing Python objects into JSON or other formats by providing built-in methods like `.dict()` or `.json()`. Similarly it facilitates deserialization by allowing you to create instances of your data models directly from JSON or other serialized formats.
 
- **Type Hinting**:
With Pydantic you can leverage Python s type hinting system to specify the expected types for each field in your data model. This not only improves code readability but also enables static type checkers like mypy to catch potential type-related issues early on.
 

In this post we will look specifically at how to use Pydantic to validate data and convert it to the correct type. We will look at a specific use case to validate API Input.  Pydantic can be used to define request/response models and automatically validate incoming requests against these models before processing them further 

###  Defining a Pydantic Model:
 To start using Pydantic you need to define a model class that represents your desired schema. Let s say we want to validate user information:

 
 ``` python
 from pydantic import BaseModel
 
 class User(BaseModel):
     name: str 
     age: int 
 ```

 Here we have defined a `User` model with two attributes: `name` (a string) and `age` (an integer). By specifying these types in our model Pydantic will automatically validate and convert input data accordingly. 
 
### Validating Input Data: 
  Once we have our model defined we can create instances of it by passing in the input data as keyword arguments: 
 
  ```python 
  user_data = { 
      "name": "John Doe" 
      "age": 25 
  } 
 
  user = User(**user_data) 
  ``` 
 
  Pydantic will validate if the provided input matches the defined schema. If any attribute is missing or has an incorrect type it will raise a `ValidationError`. This helps catch errors early on before they cause issues downstream. 
 
### Automatic Type Conversion: 
 "One of the most powerful features of Pydantic is its ability to automatically convert input data to the correct type. Let s consider an example where we have a `Product` model with a `price` attribute of type `float`:"
 
  ```python 
  class Product(BaseModel): 
      name: str 
      price: float 
 
  product_data = { 
      "name": "Example Product" 
      "price": "9.99"  # Provided as a string 
  } 
 
  product = Product(**product_data) 
  ``` 
 
  Even though the `price` is provided as a string Pydantic will automatically convert it to a float ensuring that our data is always in the expected format. 
 
  4. Handling Optional and Default Values: 
  Pydantic also allows specifying optional attributes or setting default values for attributes that may not always be present: 
 
  ```python 
  from typing import Optional 
 
  class Order(BaseModel): 
      product_id: int 
      quantity: int 
      discount: Optional[float] = None  # Optional attribute with default value 
 
  order_data = { 
      "product_id": 123 
      "quantity": 2 
  } 
 
  order = Order(**order_data) 
  ``` 
 
  In this example the `discount` attribute is optional and has been assigned a default value of `None`. If it is not provided in the input data Pydantic will automatically assign it the default value. 
 
  Conclusion: 
 "Pydantic simplifies input validation and type conversion in Python by providing an elegant way to define data schemas using classes. By leveraging Pydantic s capabilities you can ensure that your code receives valid inputs and handle any necessary type conversions seamlessly. Start using Pydantic today to enhance your data validation process and streamline your Python projects."