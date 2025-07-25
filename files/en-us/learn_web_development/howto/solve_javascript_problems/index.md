---
title: Solve common JavaScript problems
short-title: Common JavaScript problems
slug: Learn_web_development/Howto/Solve_JavaScript_problems
page-type: landing-page
sidebar: learn-how-to
---

The following links point to solutions to common problems you may encounter when writing JavaScript.

## Common beginner's mistakes

### Correct spelling and casing

If your code doesn't work and/or the browser complains that something is undefined, check that you've spelt all your variable names, function names, etc. correctly.

Some common built-in browser functions that cause problems are:

| Correct                    | Wrong                     |
| -------------------------- | ------------------------- |
| `getElementsByTagName()`   | `getElementByTagName()`   |
| `getElementsByName()`      | `getElementByName()`      |
| `getElementsByClassName()` | `getElementByClassName()` |
| `getElementById()`         | `getElementsById()`       |

### Semicolon position

You need to make sure you don't place any semicolons incorrectly. For example:

| Correct                     | Wrong                       |
| --------------------------- | --------------------------- |
| `elem.style.color = 'red';` | `elem.style.color = 'red;'` |

### Functions

There are a number of things that can go wrong with functions.

One of the most common errors is to declare the function, but not call it anywhere. For example:

```js
function myFunction() {
  alert("This is my function.");
}
```

This code won't do anything unless you call it with the following statement:

```js
myFunction();
```

#### Function scope

Remember that [functions have their own scope](/en-US/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts) — you can't access a variable value set inside a function from outside the function, unless you declared the variable globally (i.e., not inside any functions), or [return the value](/en-US/docs/Learn_web_development/Core/Scripting/Return_values) from the function.

#### Running code after a return statement

Remember also that when you return from a function, the JavaScript interpreter exits the function — no code after the return statement will run.

In fact, some browsers (like Firefox) will give you an error message in the developer console if you have code after a return statement. Firefox gives you "unreachable code after return statement".

### Object notation versus normal assignment

When you assign something normally in JavaScript, you use a single equals sign, e.g.:

```js
const myNumber = 0;
```

With [Objects](/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects), however, you need to take care to use the correct syntax. The object must be surrounded by curly braces, member names must be separated from their values using colons, and members must be separated by commas. For example:

```js
const myObject = {
  name: "Chris",
  age: 38,
};
```

## Basic definitions

- [What is JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#a_high-level_definition)
- [What is a variable?](/en-US/docs/Learn_web_development/Core/Scripting/Variables#what_is_a_variable)
- [What are strings?](/en-US/docs/Learn_web_development/Core/Scripting/Strings)
- [What is an array?](/en-US/docs/Learn_web_development/Core/Scripting/Arrays#what_is_an_array)
- [What is a loop?](/en-US/docs/Learn_web_development/Core/Scripting/Loops)
- [What is a function?](/en-US/docs/Learn_web_development/Core/Scripting/Functions)
- [What is an event?](/en-US/docs/Learn_web_development/Core/Scripting/Events)
- [What is an object?](/en-US/docs/Learn_web_development/Core/Scripting/Object_basics#object_basics)
- [What is JSON?](/en-US/docs/Learn_web_development/Core/Scripting/JSON#no_really_what_is_json)
- [What is a web API?](/en-US/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#what_are_apis)
- [What is the DOM?](/en-US/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)

## Basic use cases

### General

- [How do you add JavaScript to your page?](/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#how_do_you_add_javascript_to_your_page)
- [How do you add comments to JavaScript code?](/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#comments)

### Variables

- [How do you declare a variable?](/en-US/docs/Learn_web_development/Core/Scripting/Variables#declaring_a_variable)
- [How do you initialize a variable with a value?](/en-US/docs/Learn_web_development/Core/Scripting/Variables#initializing_a_variable)
- [How do you update a variable's value?](/en-US/docs/Learn_web_development/Core/Scripting/Variables#updating_a_variable) (also see [Assignment operators](/en-US/docs/Learn_web_development/Core/Scripting/Math#assignment_operators))
- [What data types can values have in JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/Variables#variable_types)
- [What does 'loosely typed' mean?](/en-US/docs/Learn_web_development/Core/Scripting/Variables#dynamic_typing)

### Math

- [What types of number do you have to deal with in web development?](/en-US/docs/Learn_web_development/Core/Scripting/Math#types_of_numbers)
- [How do you do basic math in JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/Math#arithmetic_operators)
- [What is operator precedence, and how is it handled in JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/Math#operator_precedence)
- [How do you increment and decrement values in JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/Math#increment_and_decrement_operators)
- [How do you compare values in JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/Math#comparison_operators) (e.g., to see which one is bigger, or to see if one value is equal to another).

### Strings

- [How do you create a string in JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/Strings#declaring_strings)
- [Do you have to use single quotes or double quotes?](/en-US/docs/Learn_web_development/Core/Scripting/Strings#single_quotes_double_quotes_and_backticks)
- [How do you join strings together?](/en-US/docs/Learn_web_development/Core/Scripting/Strings#concatenation_in_context)
- [Can you join strings and numbers together?](/en-US/docs/Learn_web_development/Core/Scripting/Strings#numbers_vs._strings)
- [How do you find the length of a string?](/en-US/docs/Learn_web_development/Core/Scripting/Useful_string_methods#finding_the_length_of_a_string)
- [How do you find what character is at a certain position in a string?](/en-US/docs/Learn_web_development/Core/Scripting/Useful_string_methods#retrieving_a_specific_string_character)
- [How do you find and extract a specific substring from a string?](/en-US/docs/Learn_web_development/Core/Scripting/Useful_string_methods#extracting_a_substring_from_a_string)
- [How do you change the case of a string?](/en-US/docs/Learn_web_development/Core/Scripting/Useful_string_methods#changing_case)
- [How do you replace one specific substring with another?](/en-US/docs/Learn_web_development/Core/Scripting/Useful_string_methods#updating_parts_of_a_string)

### Arrays

- [How do you create an array?](/en-US/docs/Learn_web_development/Core/Scripting/Arrays#creating_arrays)
- [How do you access and modify the items in an array?](/en-US/docs/Learn_web_development/Core/Scripting/Arrays#accessing_and_modifying_array_items) (this includes multidimensional arrays)
- [How do you find the length of an array?](/en-US/docs/Learn_web_development/Core/Scripting/Arrays#finding_the_length_of_an_array)
- [How do you add items to an array?](/en-US/docs/Learn_web_development/Core/Scripting/Arrays#adding_items)
- [How do you remove items from an array?](/en-US/docs/Learn_web_development/Core/Scripting/Arrays#removing_items)
- [How do you split a string into array items, or join array items into a string?](/en-US/docs/Learn_web_development/Core/Scripting/Arrays#converting_between_strings_and_arrays)

### Debugging JavaScript

- [What are the basic types of error?](/en-US/docs/Learn_web_development/Core/Scripting/What_went_wrong#types_of_error)
- [What are browser developer tools, and how do you access them?](/en-US/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools)
- [How do you log a value to the JavaScript console?](/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#the_console_api)
- [How do you use breakpoints and other JavaScript debugging features?](/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript#using_the_javascript_debugger)

For more information on JavaScript debugging, see [JavaScript debugging and error handling](/en-US/docs/Learn_web_development/Core/Scripting/Debugging_JavaScript). Also, see [Other common errors](/en-US/docs/Learn_web_development/Core/Scripting/What_went_wrong#other_common_errors) for a description of common errors.

### Making decisions in code

- [How do you execute different blocks of code, depending on a variable's value or other condition?](/en-US/docs/Learn_web_development/Core/Scripting/Conditionals)
- [How do you use if ...else statements?](/en-US/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements)
- [How do you nest one decision block inside another?](/en-US/docs/Learn_web_development/Core/Scripting/Conditionals#nesting_if...else)
- [How do you use AND, OR, and NOT operators in JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/Conditionals#logical_operators_and_or_and_not)
- [How do you conveniently handle a large number of choices for one condition?](/en-US/docs/Learn_web_development/Core/Scripting/Conditionals#switch_statements)
- [How do you use a ternary operator to make a quick choice between two options based on a true or false test?](/en-US/docs/Learn_web_development/Core/Scripting/Conditionals#ternary_operator)

### Looping/iteration

- [How do you run the same bit of code over and over again?](/en-US/docs/Learn_web_development/Core/Scripting/Loops)
- [How do you exit a loop before the end if a certain condition is met?](/en-US/docs/Learn_web_development/Core/Scripting/Loops#exiting_loops_with_break)
- [How do you skip to the next iteration of a loop if a certain condition is met?](/en-US/docs/Learn_web_development/Core/Scripting/Loops#skipping_iterations_with_continue)
- [How do you use while and do...while loops?](/en-US/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while)

## Intermediate use cases

### Functions

- [How do you find functions in the browser?](/en-US/docs/Learn_web_development/Core/Scripting/Functions#built-in_browser_functions)
- [What is the difference between a function and a method?](/en-US/docs/Learn_web_development/Core/Scripting/Functions#functions_versus_methods)
- [How do you create your own functions?](/en-US/docs/Learn_web_development/Core/Scripting/Build_your_own_function)
- [How do you run (call, or invoke) a function?](/en-US/docs/Learn_web_development/Core/Scripting/Functions#invoking_functions)
- [What is an anonymous function?](/en-US/docs/Learn_web_development/Core/Scripting/Functions#anonymous_functions_and_arrow_functions)
- [How do you specify parameters (or arguments) when invoking a function?](/en-US/docs/Learn_web_development/Core/Scripting/Functions#function_parameters)
- [What is function scope?](/en-US/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)
- [What are return values, and how do you use them?](/en-US/docs/Learn_web_development/Core/Scripting/Return_values)

### Objects

- [How do you create an object?](/en-US/docs/Learn_web_development/Core/Scripting/Object_basics#object_basics)
- [What is dot notation?](/en-US/docs/Learn_web_development/Core/Scripting/Object_basics#dot_notation)
- [What is bracket notation?](/en-US/docs/Learn_web_development/Core/Scripting/Object_basics#bracket_notation)
- [How do you get and set the methods and properties of an object?](/en-US/docs/Learn_web_development/Core/Scripting/Object_basics#setting_object_members)
- [What is `this`, in the context of an object?](/en-US/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this)
- [What is object-oriented programming?](/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming)
- [What are constructors and instances, and how do you create them?](/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#classes_and_instances)

### JSON

- [How do you structure JSON data, and read it from JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/JSON#json_structure)
- [How do you convert a JSON object to a text string, and back again?](/en-US/docs/Learn_web_development/Core/Scripting/JSON#converting_between_objects_and_text)

### Events

- [What are event handlers and how do you use them?](/en-US/docs/Learn_web_development/Core/Scripting/Events#event_handler_properties)
- [What are inline event handlers?](/en-US/docs/Learn_web_development/Core/Scripting/Events#inline_event_handlers_—_dont_use_these)
- [What does the `addEventListener()` function do, and how do you use it?](/en-US/docs/Learn_web_development/Core/Scripting/Events#using_addeventlistener)
- [What are event objects, and how do you use them?](/en-US/docs/Learn_web_development/Core/Scripting/Events#event_objects)
- [How do you prevent default event behavior?](/en-US/docs/Learn_web_development/Core/Scripting/Events#preventing_default_behavior)
- [How do events fire on nested elements? (event propagation, also related — event bubbling and capturing)](/en-US/docs/Learn_web_development/Core/Scripting/Event_bubbling)
- [What is event delegation, and how does it work?](/en-US/docs/Learn_web_development/Core/Scripting/Event_bubbling#event_delegation)

### Object-oriented JavaScript

- [What are object prototypes?](/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes)
- [How do you add methods to the constructor?](/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes#setting_a_prototype)
- [How do you create a new constructor that inherits its members from a parent constructor?](/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript)
- [When should you use inheritance in JavaScript?](/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#inheritance)

### Web APIs

- [How do you manipulate the DOM (e.g., adding or removing elements) using JavaScript?](/en-US/docs/Learn_web_development/Core/Scripting/DOM_scripting#doing_some_basic_dom_manipulation)
