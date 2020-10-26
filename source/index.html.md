---
title: Ambulatory Assessment Components Reference

language_tabs: # must be one of https://git.io/vQNgJ


toc_footers:
  # - <a href='#'>Sign Up for a Developer Key</a>
  # - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---





# Introduction

Welcome to the Ambulatory Assessment Components Reference! Ambulatory Assessment Components consist of two parts, executable HTML Components and Ambulatory Assessment Widget Components.


Ambulatory Assessment Widget Components help the author construct interfaces for response items in questionnaires


# executable HTML

Executable HTML Components help the author of HTML content prescribe the way pieces of HTML are inserted into the document, without necessitating the use of JavaScript by the author.




















## aa-session

```html
<aa-session>
  <template>

  <!-- all content goes here !-->

  </template>
</aa-session>
```

 ``aa-session`` is the container element for the whole ambulatory assessment protocol. It should always have a ``template`` as an immediate child. ``template`` allows the browser to ignore the protocol content, so that ``aa-session`` can parse
 it and initalize it appropriately, for example where components like ```aa-sequence``` and ```aa-choose``` are used.

``aa-session`` dispatches the ``sessionReady`` event to signal that its initialization is complete. Then, children of ``aa-session`` have also been initialized and have been inserted in the document. This is illustrated in the following example,that discussess our next element, ``aa-sequence``

#### attributes

__``name``__: ``string`` : A name to represent the values generated from the element

__``diagram``__: ``boolean`` : Set it to ``true`` to generate a drawing of the session's contents, instead of running the session

__``should-run``__: ``boolean`` : Set it to ``false`` to prevent the session from initializing automatically when the page is loaded


#### methods

__``.run()``__:  If ``should-run`` has been set to ``false``, call this method to get the session started.


#### events

__``sessionReady``__:  Dispatched when ``aa-session`` has finished initializing. 


















## aa-sequence


<!-- ``aa-sequence`` is an element that implements sequential execution. -->

``aa-sequence`` implements sequential insertion of each of its children into the document. Insertion of the next child can be triggered either by an event, or manually by calling its member method ``.next()``. 



```html
<aa-session>
<template>
  <aa-sequence id="sequence1">
    <div> item 1 </div>
    <div> item 2 </div>
    <div> item 3 </div>
  </aa-sequence>

  <paper-button id='button1' style="margin-top:20px"> next </paper-button>
  </template>
</aa-session>

<script> 
  window.addEventListener("sessionReady", ()=>{
    document.getElementById('button1')
            .addEventListener('click', ()=>{ document.getElementById('sequence1').next(); })
  })
</script>

```


<p id="container1">This example illustrates triggering a sequence through a button. We listen for the <code>sessionReady</code> event to make sure <code>#button1</code> has been inserted in the document, and set a click event listener, that calls the <code>.next()</code> member function of <code>#sequence1</code>. Pressing the next" button adds content as per the sequence.</p>


<!-- The following is a live example of a working sequence, which I want to be part of the natural
flow of the documentation, appearing next to its corresponding source, so I have to place it
in the same container, #container1, manually
  -->

<script> 
let src1=`

<aa-session id="session1">
<template>


  <aa-sequence id="sequence1">

    <div> item 1 </div>
    <div> item 2 </div>
    <div> item 3 </div>
  </aa-sequence>

  <paper-button id='button1' style="margin-top:20px"> next </paper-button>
</template>
</aa-session>`

setTimeout(function(){
  let d = document.createElement('p');
  d.style.marginTop="20px";
  
  document.querySelector('#container1').appendChild(d);
  d.innerHTML = src1;
  let b = document.getElementById("button1").addEventListener("click", function(){
    let s = document.getElementById("sequence1").next();

  })
},100);
</script>


The sequence can also listen for an event called ``endEvent``, which its children can dispatch to let it know that it should insert its next element.

``aa-screen`` is an element that simplifies managing ``aa-sequence`` in user interactions.

#### attributes

__``name``__: ``string`` : A name to represent the values generated from the element

__``diagram``__: ``boolean`` : Set it to true to generate a drawing of the session's contents, instead of running the session

__``should-run``__: ``boolean`` : Set it to ``false`` to prevent the sequence from executing once it is added to the document

#### methods

__``.next()``__: Calling it will cause the sequence to progress by one step, and insert the next child element in line, into the document

#### events

__``endEvent``__ : Dispatched when `aa-sequence`` has reched the end.








## aa-variable


```html
<aa-session>
  <template>

    <aa-variable name="myVariable" value="myValue"></aa-variable>

  </template>
</aa-session>
```


``aa-variable`` allows the html author to declare named variables and assign values to them. Each variable and its value is stored in LocalStorage, and is available to all other aa-elements as well.

#### attributes

__``name``__: ``string`` : A name to represent the values generated from the element

__``diagram``__: ``boolean`` : Set it to true to generate a drawing of the session's contents, instead of running the session

__``value``__: ``string | number | boolean`` : The value that the variable holds













## aa-function- *


A set of functions allow the author to assign the result of a function to a named variable.

### aa-function-random

```html
<aa-session>
  <template>

    <aa-function-random name="myRandomVariable" min=1 max=100></aa-variable>

  </template>
</aa-session>
```

``aa-fuction-random`` generates a random integer between its ``min`` and ``max`` attributes and assigns it to a variable with the name of its ``name`` attribute


#### attributes

__``name``__: ``string`` : A name to represent the values generated from the element

__``diagram``__: ``boolean`` : Set it to true to generate a drawing of the session's contents, instead of running the session

__``min``__: ``number`` : The lower boundary of the random number to be generated

__``max``__: ``number`` : The upper boundary of the random number to be generated









## aa-choose, aa-when, aa-otherwise


```html
<aa-session>
  </template>

    <aa-function-random name="myVar" min=1 max=10></aa-fuction-random>
    
    <aa-choose>
      <aa-when test="myVar==1">
        content to insert when myVar equals 1
      </aa-when>

      <aa-when test="myVar==2">
        content to insert when myVar equals 2
      </aa-when>

      <aa-otherwise>
        content to insert if none of the aa-when conditions are true
      </aa-otherwise>
    </aa-choose>
  
  </template>
</aa-session>
```
``aa-choose`` alongside, ``aa-when`` and ``aa-otherwise`` implement conditional execution. 

``aa-choose` is the container element for `aa-when`` and ``aa-otherwise``. There is no limit to the number of ``aa-when`` children it can contain, but it should only have ``aa-otherwise`` child element.

Each ``aa-when`` has a conditional statement in its ``test`` attribute, which evaluates to either true or false. All ``aa-when`` nodes which have a ``test`` condition that evaluates to true are inserted into the document.


In the example on the right, a random number between 1 and 10 is generated and stored in variable ``myVar``. The ``aa-choose`` node contains 2 ``aa-when`` children. If ``myVar`` equals 1, the first  one will be inserted. If ``myVar`` equals 2, the second one will be inserted. In all other cases, the ``aa-otherwise`` child will be inserted


#### aa-when attributes

__``test``__: ``string`` : A conditional statement that is evaluated by the ``aa-choose`` parent node. Conditional statements can include logical operators, numbers and strings, and also variable names that have already been declared.












## aa-screen


 ``aa-screen`` is a container that can group multiple elements. It includes a "next" button, which, when clicked, dispatches an ``endEvent`` so that a sequence can insert its next element.
 
 ```html
 <aa-session>
  <template>

    <aa-sequence>
      <aa-screen submit-button-text="next"> screen1</aa-screen>
      <aa-screen submit-button-text="next"> screen2</aa-screen>
      <aa-screen submit-button-text="next"> screen3</aa-screen>
    </aa-sequence>

  </template>
</aa-session>
```

<p id="container2">This example illustrates a sequence with 3 screens</p>


<script>
 let src2=`<aa-session>
  <template>

    <aa-sequence>
      <aa-screen submit-button-text="next"> screen1</aa-screen>
      <aa-screen submit-button-text="next"> screen2</aa-screen>
      <aa-screen submit-button-text="next"> screen3</aa-screen>
    </aa-sequence>

  </template>
</aa-session>`

setTimeout(function(){
  let d = document.createElement('p');
  d.style.marginTop="20px";
  
  document.querySelector('#container2').appendChild(d);
  d.innerHTML = src2;
},100);
</script>


``aa-screen`` will also collect values from all child elements, and dispatch them in a ``valueSubmit`` event



#### attributes

__``submit-button-text``__: ``string`` : The label for the `aa-screen`` button, that causes it to progress by one step. Its default label is "submit" (derived from the action of submitting the response), but it can be set to any other prompt fro the user.

__``submit-button-hidden``__: ``boolean`` : Set it to true to cause the ``aa-screen`` button to not be diplayed. Useful for ``aa-screen`` nodes at the end of a sequence.

#### events

__``valueSubmit``__ : Dispatched when the `aa-screen`` button has been clicked. 













# Widget Components


























```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```


<aside class="notice">
testYou must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens



```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

