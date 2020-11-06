---
title: Executable HTML and Ambulatory Assessment Components Reference

language_tabs: # must be one of https://git.io/vQNgJ


toc_footers:
  # - <a href='#'>Sign Up for a Developer Key</a>
  # - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---






<br><br>
Welcome to the Ambulatory Assessment Components Reference! 

Ambulatory Assessment Components consist of two parts, executable HTML Components and Ambulatory Assessment Widget Components.

Ambulatory Assessment Widget Components help the author construct interfaces for response items in questionnaires.


# Ambulatory Assessment

Ambulatory Assessment is a general term that refers to a class of methods used in clinical psychology (and other fields), to collect data from a group of people (participants in the study). These methods share the following characteristics:
 
 - They take place over a period of time
 - The same type of data are collected repeatedly, sometimes several times a day
 - Types of data can be either questionnaires that participants answer directly, or they can be measurements captured automatically via sensors (e.g, a heart-rate sensor on a wearable device)
 - The data collection takes place within the regular daily life of participants, as opposed to a laboratory setting.

Nowadays, there is great interest in conducting and monitoring such studies with software, on mobile devices, such as smartphones and smartwatches. The Web is a useful medium for implementing a significant part of the work involved in building such software, so a small HTML primer follows.




# HTML primer

HTML stands for __Hyper-Text Markup Language__. 

__Hyper-Text__ stands for text displayed on computers, which contains links to other text. Such is the nature of the Web pages that we all know. 

__Markup__ means a way to annotate parts of the text, so that the computer knows that they should be displayed in a particular way. 

__Language__ means that there exists a specific set of HTML elements to choose from,
and that a certain syntax is used to represent them. 



## HyperText Markup


For example, Take a look at these two lines of text.

<p class="sticktoexample"></p>

```html
Wikipedia is at www.wikipedia.org

Wikipedia is <a href="http://www.wikipedia.org">here</a>
```

<p class="myexample">
  Wikipedia is at www.wikipedia.org
  <br><br>
  Wikipedia is  <a href="www.wikipedia.org">here</a>
</p>


The first line contains a simple mention of a url. The second one contains themarkup ``<a href="...">here</a>`` that tells the computer to display the content( the word ``here`` ) of that markup (``<a>``) as a clickable link, that links to the url contained in the attribute ``href``.  ``a`` stands for Anchor. An anchor is a piece of text which marks the beginning and/or the end of a hypertext link.

So in this case, the anchor instructs the browser to display what would otherwise be plain text, as a functional element of the interface.



## Tags, Attributes, Elements

In HTML, the annotation syntax is called __HTML tag__, which consists of two parts; an __*opening tag*__, e.g., __``<a>``__  and a corresponding __*closing tag*__, e.g, __``</a>``__. The part of the text that consists of an opening tag, its corresponding closing tag, and the contents inbetween is called an __HTML element__. The HTML tag marks the start and end of the HTML element. The text inbetween  the start and end tag is the content. As such, sometimes an element is referred to as a container.

An opening tag can also contain __attributes__. Attributes further define details about the element, e.g. in ``<a href="htttp://www.wikipedia.org>here</a>``, the attribute ``href`` defines the url, which this anchor element links to.



## Parents and Children


<!-- #### Further examples


__Paragraph ``<p>`` :  ``<> -->


# Executable HTML Components

Executable HTML Components help the author of HTML content prescribe the way pieces of HTML are inserted into the document, without necessitating the use of JavaScript by the author.




















## aa-session


 ``aa-session`` is the container element for the whole ambulatory assessment protocol. It should always have a ``template`` as an immediate child. ``template`` allows the browser to ignore the protocol content, so that ``aa-session`` can parse
 it and initalize it appropriately, for example where components like ```aa-sequence``` and ```aa-choose``` are used.

``aa-session`` dispatches the ``sessionReady`` event to signal that its initialization is complete. Then, children of ``aa-session`` have also been initialized and have been inserted in the document. This is illustrated in the following example,that discussess our next element, ``aa-sequence``


```html
<aa-session>
  <template>

  <!-- all content goes here !-->

  </template>
</aa-session>
```




 attribute | type   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__| ``string`` | A name to represent the values generated from the element
__``diagram``__| ``boolean`` | Set it to ``true`` to generate a drawing of the session's contents, instead of running the session
__``should-run``__| ``boolean`` | Set it to ``false`` to prevent the session from initializing automatically when the page is loaded


 method | returns   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``.run()``__| ``undefined`` |  If ``should-run`` has been set to ``false``, call this method to get the session started.


 event |   |
|--------------|:-------------|:----------------------------------------------------------|
__``sessionReady``__|  Dispatched when ``aa-session`` has finished initializing. 


















## aa-sequence


<!-- ``aa-sequence`` is an element that implements sequential execution. -->

``aa-sequence`` implements sequential insertion of each of its children into the document. Insertion of the next child can be triggered either by an event, or manually by calling its member method ``.next()``. 

This example illustrates triggering a sequence through a button. We listen for the ``sessionReady`` event to make sure ``#button1 has been inserted in the document, and set a click event listener, that calls the ``.next()`` member function of ``#sequence1``. Pressing the next" button adds content as per the sequence.



<p class="sticktoexample"></p>

```html
<aa-session>
  <template>

    <aa-sequence id="sequence1">
      <div> item 1 </div>
      <div> item 2 </div>
      <div> item 3 </div>
    </aa-sequence>

    <paper-button id='button1'>
      next
    </paper-button>

  </template>
</aa-session>

<!-- lets make the button move aa-sequence forward -->
<script> 
  window.addEventListener("sessionReady", ()=>{
    document.getElementById('button1')
            .addEventListener('click', ()=>{ 
              document.getElementById('sequence1').next();
            })
  })
</script>
```


<p id="container1" class="myexample"></p>


<!-- The following is a live example of a working sequence, which I want to be part of the natural
flow of the documentation, appearing next to its corresponding source, so I have to place it
in the same container, #container1, manually
  -->

<script> 
let src1=`

<aa-session id="session1" class="myexample">
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
  d.style.maxWidth="500px";
  d.style.marginTop="20px";
  
  document.querySelector('#container1').appendChild(d);

  d.innerHTML = src1;
  d.addEventListener("sessionReady", function(){
    let b = document.getElementById("button1").addEventListener("click", function(){
      let s = document.getElementById("sequence1").next();
  })
  
  

  })
},100);
</script>


The sequence can also listen for an event called ``endEvent``, which its children can dispatch to let it know that it should insert its next element.

We can ignore the `<script>` section of the example for now. [``aa-screen``](#aa-screen), discussed below, is an element that simplifies managing an ``aa-sequence`` in user interactions.


 attribute | type   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__| ``string`` | A name to represent the values generated from the element
__``diagram``__| ``boolean`` | Set it to true to generate a drawing of the session's contents, instead of running the session
__``should-run``__| ``boolean`` | Set it to ``false`` to prevent the sequence from executing once it is added to the document


 method | returns   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``.next()``__ | ``undefined`` | Calling it will cause the sequence to progress by one step, and insert the next child element in line, into the document


 event |   |
|--------------|:----------------------------------------------------------|
__``endEvent``__ | Dispatched when `aa-sequence`` has reached the end.




## aa-jump

The ``aa-jump`` element is a child of ``aa-sequence``. An author can use it to instruct the sequence to continue its execution from a specific child. 


 attribute | type   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__| ``string`` | A name to represent the element 
__``goto``__| ``string`` | The name of the child of the ``aa-sequence`` that will be next.





## aa-variable

``aa-variable`` allows the html author to declare named variables and assign values to them. Each variable and its value is stored in LocalStorage, and is available to all other aa-elements as well.

```html
<aa-session>
  <template>

    <aa-variable name="myVariable" value="myValue">
    </aa-variable>

  </template>
</aa-session>
```






 attribute | type   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__ | ``string`` | A name to represent the values generated from the element |
__``diagram``__ | ``boolean`` | Set it to true to generate a drawing of the session's contents, instead of running the session |
__``value``__ | ``string``  ``number``  ``boolean`` | The value that the variable holds |













## aa-function- *


A set of functions allow the author to assign the result of a function to a named variable. For now, only ``aa-function-random`` exists.


### aa-function-random

``aa-fuction-random`` generates a random integer between its ``min`` and ``max`` attributes and assigns it to a variable with the name of its ``name`` attribute

```html
<aa-session>
  <template>

    <aa-function-random name="myRandomVariable" min=1 max=100>
    </aa-function-random>

  </template>
</aa-session>
```







 attribute | type   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__ | ``string``   | A name to represent the values generated from the element |
__``diagram``__ | ``boolean``   | Set it to true to generate a drawing of the session's contents, instead of running the session |
__``min``__ | ``number`` | The lower boundary of the random number to be generated |
__``max``__ | ``number`` | The upper boundary of the random number to be generated |











## aa-choose, aa-when, aa-otherwise


```html
<aa-session>
  </template>

    <aa-function-random name="myVar" min=1 max=10>
    </aa-fuction-random>
    
    <aa-choose>
      <aa-when test="myVar==1">
        content to insert when myVar equals 1
      </aa-when>

      <aa-when test="myVar==2">
        content to insert when myVar equals 2
      </aa-when>

      <aa-otherwise>
        content to insert 
        if none of the aa-when conditions are true
      </aa-otherwise>
    </aa-choose>
  
  </template>
</aa-session>
```
``aa-choose`` alongside, ``aa-when`` and ``aa-otherwise`` implement conditional execution. 

``aa-choose` is the container element for `aa-when`` and ``aa-otherwise``. There is no limit to the number of ``aa-when`` children it can contain, but it should only have ``aa-otherwise`` child element.

Each ``aa-when`` has a conditional statement in its ``test`` attribute, which evaluates to either true or false. All ``aa-when`` nodes which have a ``test`` condition that evaluates to true are inserted into the document.


In the example on the right, a random number between 1 and 10 is generated and stored in variable ``myVar``. The ``aa-choose`` node contains 2 ``aa-when`` children. If ``myVar`` equals 1, the first  one will be inserted. If ``myVar`` equals 2, the second one will be inserted. In all other cases, the ``aa-otherwise`` child will be inserted


aa-when attribute | type   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``test``__| ``string`` | A conditional statement that is evaluated by the ``aa-choose`` parent node. Conditional statements can include logical operators, numbers and strings, and also variable names that have already been declared.












## aa-screen



 ``aa-screen`` is a container that can group multiple elements. It includes a "next" button, which, when clicked, dispatches an ``endEvent`` so that a sequence can insert its next element. ``aa-screen`` will also collect values from all child elements, and dispatch them in a ``valueSubmit`` event
 
 ```html
 <aa-session>
  <template>

    <aa-sequence>
      <aa-screen submit-button-text="next"> 
        This is a first screen. 
        Press next to go to the second one.
      </aa-screen>
      <aa-screen submit-button-text="next"> 
        This is a second screen. 
        Press next to go to the third and final one.
      </aa-screen>
      <aa-screen submit-button-hidden>
        This is the third and final screen. 
        Note that the button is now hidden.
      </aa-screen>
    </aa-sequence>

  </template>
</aa-session>
```


<p id="container2" class="myexample">This example illustrates a sequence with 3 screens:</p>


<script>
 let src2=`<aa-session>
  <template>

    <aa-sequence>
       <aa-screen submit-button-text="next"> This is a first screen. Press next to go to the second one.</aa-screen>
      <aa-screen submit-button-text="next"> This is a second screen. Press next to go to the third and final one.</aa-screen>
      <aa-screen submit-button-hidden> This is the third and final screen. Note that the button is now hidden.</aa-screen>
    </aa-sequence>

  </template>
</aa-session>`

setTimeout(function(){
  let d = document.createElement('p');
  // d.style.maxWidth="500px";
  d.style.marginTop="20px";
  
  document.querySelector('#container2').appendChild(d);
  d.innerHTML = src2;
},100);
</script>






 attribute | type   |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__| ``string`` | A name to represent the values generated from the element.
__``diagram``__| ``boolean`` | Set it to true to generate a drawn representation of the screen.
__``submit-button-text``__| ``string`` | The label for the `aa-screen`` button, that causes it to progress by one step. Its default label is "submit" (derived from the action of submitting the response), but it can be set to any other prompt fro the user.
__``submit-button-hidden``__| ``boolean`` | Set it to true to cause the ``aa-screen`` button to not be diplayed. Useful for ``aa-screen`` nodes at the end of a sequence.


 event |   |
|--------------|:-------------|:----------------------------------------------------------|
__``valueSubmit``__ |Dispatched when the `aa-screen`` button has been clicked. 












# Widget Components




## aa-affect-grid


The Affect Grid is a scale designed as a quick means of assesing affect along two dimensions. Its implementation as ``<aa-affect-grid>`` allows the author to label boundaries along the x and y axes, as well as the number of rows and columns. ``<aa-affect-grid>`` scales to the width of its container.


```html
<aa-affect-grid 
  top-left-label="top-left" 
  top-label="top" 
  top-right-label="top-right" 
  
  left-top-label="left-top" 
  right-top-label="right-top" 
  
  left-label="left" 
  right-label="right" 

  left-bottom-label="left-bottom" 
  right-bottom-label="right-bottom" 
  
  bottom-left-label="bottom-left" 
  bottom-label="bottom" 
  bottom-right-label="bottom-right"
  
  rows="11"
  columns="11">
</aa-affect-grid>
```



<p  class="myexample">
click on the grid to see how the value of the response item changes:
<br><br>
<aa-affect-grid id="grid1" top-left-label="top-left" top-label="top" top-right-label="top-right" left-top-label="left-top" right-top-label="right-top" left-label="left" center-label="center" right-label="right" left-bottom-label="left-bottom" right-bottom-label="right-bottom" bottom-left-label="bottom-left" bottom-label="bottom" bottom-right-label="bottom-right"></aa-affect-grid>

<br>
<b>current aa-affect-grid value</b>: <span id="grid1value"></span>
</p>






<script>
  let grid = document.getElementById("grid1");
  let vgrid1 = document.getElementById("grid1value");
  vgrid1.innerHTML = grid.value;  
  grid.addEventListener("change", function(e){
    vgrid1.innerText = JSON.stringify(grid.value);
  })

</script>


 attribute | type  |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__| ``string`` | A name to represent the values generated from the element.
__``top-left-label``__ __``top-label``__   __``top-right-label``__ __``left-top-label``__, __``right-top-label``__ __``left-label``__  __``right-label``__ __``left-bottom-label``__ __``right-bottom-label``__ __``bottom-left-label``__,  __``bottom-label``__, __``bottom-right-label``__  | ``string`` | Labels for various parts of the grid.
__``rows``__| ``number`` | Number of grid rows. Default value is 11   
__``columns``__| ``number`` | Number of grid columns. Default value is 11
__value__| ``object`` | The x,y coordinates of the grid selection.


 event |   |  
|--------------|:-------------|:----------------------------------------------------------|
__``change``__| Dispatched when a different selection is made in ``aa-affect-grid``





















## aa-checkboxes

__``aa-checkboxes``__ implements a multiple response item, where each available choice is specified as a [``aa-choice-item``](#aa-choice-item).

In the following configuration, the text inside each available choice is used as the value that each `aa-choice-item` corresponds to.

```html
<aa-checkboxes name="checkboxes1">
  <aa-choice-item> choice 1 </aa-choice-item>
  <aa-choice-item> choice 2 </aa-choice-item>
  <aa-choice-item> choice 3 </aa-choice-item>
</aa-checkboxes>
```

<p class="myexample">
click on checkboxes to see how the response item behaves
<aa-checkboxes id="checkboxes1">
  <aa-choice-item> choice 1 </aa-choice-item>
  <aa-choice-item> choice 2 </aa-choice-item>
  <aa-choice-item> choice 3 </aa-choice-item>
</aa-checkboxes>

<br>
<b>current checkboxes value</b> : <span id="checkboxes1value"></span>
</p>

It is possible to also specify the value that each [``aa-choice-item``](#aa-choice-item) corresponds to. The following example shows a set of
checkboxes where values for each `aa-choice-item`` have been set. It also shows how the ``horizontal`` attribute is used.

```html
<aa-checkboxes name="checkboxes2" horizontal>
  <aa-choice-item value="A"> choice 1 </aa-choice-item>
  <aa-choice-item value="B"> choice 2 </aa-choice-item>
  <aa-choice-item value="C"> choice 3 </aa-choice-item>
</aa-checkboxes>
```

<p class="myexample">
<aa-checkboxes id="checkboxes2" horizontal>
    <aa-choice-item value="A"> choice 1 </aa-choice-item>
  <aa-choice-item value="B"> choice 2 </aa-choice-item>
  <aa-choice-item value="C"> choice 3 </aa-choice-item>
</aa-checkboxes>

<br>

</p>
<script>

  let checkboxes1 = document.getElementById("checkboxes1");
  let vcheckboxes1 = document.getElementById("checkboxes1value");
  vcheckboxes1.innerHTML = checkboxes1.value;
  checkboxes1.addEventListener("change", function(e){
    console.log(checkboxes1.value);
    vcheckboxes1.innerText = JSON.stringify(checkboxes1.value);
  })
</script>

 attribute | type  |  |
|--------------|:-------------|:----------------------------------------------------------|
__``horizontal``__ | ``boolean`` | Places the checkboxes in a horizontal layout
__``name``__ | ``string`` | A name to represent the values generated from the element.
__``value``__ | ``any`` | An array of the values of the checkboxes that were ticked. Unticked checkboxes are represented by null values.


 event |  |
|--------------|:-------------|:----------------------------------------------------------|
__``change``__ | Dispatched when a selection in the group of choices changes.
















## aa-choice-item

``<aa-choice-item>`` is an element that implements an available choice for ``<aa-checkboxes>`` and ``<aa-multiple-choice>``. The value corresponding to the chosen item is specified by the ``value`` attribute. If no ``value`` attribute is supplied, the content of the element is taken as its value.



This example demonstrates the use of the ``value`` attribute vs no ``value`` specified. The first ``aa-choice-item`` emits its content as a value, because no ``value`` attribute is specified. The second one, emits the specified ``value`` attribute as its value.

```html
<aa-checkboxes>
  <aa-choice-item> choice 1 </aa-choice-item>
  <aa-choice-item value="2"> choice 2 </aa-choice-item>
</aa-checkboxes>
```



<p class="myexample">
click on checkboxes to see how the value of the response item changes:
<aa-checkboxes id="checkboxes2">
  <aa-choice-item> choice 1 </aa-choice-item>
  <aa-choice-item value="2"> choice 2 </aa-choice-item>
</aa-checkboxes>
<br>
<b>current checkboxes value</b> : <span id="checkboxes2value"></span>
</p>


<script>

  let checkboxes2 = document.getElementById("checkboxes2");
  let vcheckboxes2 = document.getElementById("checkboxes2value");
  vcheckboxes2.innerHTML = checkboxes2.value;
  checkboxes2.addEventListener("change", function(e){
    console.log(checkboxes2.value);
    vcheckboxes2.innerText = JSON.stringify(checkboxes2.value);
  })
</script>



 attribute | type  |  |
|--------------|-------------|----------------------------------------------------------|
__``value``__| ``any`` | A value that corresponds to choosing this element. If no value is provided, its content is taken as its value.













## aa-geolocation

``<aa-geolocation>`` queries the browser for the user's GPS location. As a result, a permission request might be triggered to access the user's location.

To use it, place it in an [``aa-screen``](#aa-screen) element, as any other widget. The user will be asked for their permission to get their location. 

```html
<aa-geolocation name="mylocation"></aa-geolocation>
```




 attribute | type  |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__| ``string`` | A name to represent the values generated from the element.
__``value``__| ``any`` | The user's longitude and latitude as provided by the browser.







## aa-label

The question the user is answering with the response item is oftentimes called "label". Though it is not necessary for the author to use this element, it is provided as a way to annotate the text that specifically serves this purpose, so an author might write for example:``<aa-label>How do you feel?</aa-label>`` to let others that process the html protocol, know that ``How do you feel`` is a label. It styles its content as bold.

```html
<aa-label>How do you feel?</aa-label>
<aa-multiple-choice name="feeling">
  <aa-choice-item value="1">not okay</aa-choice-item>
  <aa-choice-item value="2">just okay</aa-choice-item>
  <aa-choice-item value="3">very okay</aa-choice-item>
</aa-multiple-choice>
```

<p class="myexample">
<aa-label>How do you feel?</aa-label>
<aa-multiple-choice name="feeling">
  <aa-choice-item value="1">not okay</aa-choice-item>
  <aa-choice-item value="2">just okay</aa-choice-item>
  <aa-choice-item value="3">very okay</aa-choice-item>
</aa-multiple-choice>
</p>








## aa-likert-scale

``aa-likert-scale`` is a preconfigured [``aa-multiple-choice``](#aa-multiple-choice) item that implements a n-item rating scale. It also supports labels at start, middle and end, and allows the author to specify the number at which rating starts


<p class="sticktoexample"></p>
```html
<aa-label>scale 1:</aa-label>
<aa-likert-scale name="my5itemScale" items="5"
  start-label="start" 
  middle-label="middle" 
  end-label="end" 
  start-item="0"
  ></aa-likert-scale>
```

<p class="myexample" id="aaLikertScaleContainer">

<aa-label>scale1:</aa-label>
  <aa-likert-scale id="likert1" name="my5itemScale" start-label="start" middle-label="middle" end-label="end" items="5" start-item="0"></aa-likert-scale>
  <br>
  <b>value of scale 1:</b> <span id="likert1value"></span>
  <br>
</p>


<p class="sticktoexample"></p>

```html
<aa-label>scale 2:</aa-label>
<aa-likert-scale items="7" name="my7itemScale"></aa-likert-scale>
```

<p class="myexample">
  <aa-label>scale 2:</aa-label>
  <aa-likert-scale id="likert2" items="7"></aa-likert-scale>
  <br>
<b>value of scale 2:</b> <span id="likert2value"></span>
</p>


<script>
  let likert1 = document.getElementById("likert1");
  let likert2 = document.getElementById("likert2");
  let likert1value = document.getElementById("likert1value");
  let likert2value = document.getElementById("likert2value");

  likert1.addEventListener("change", function(){
    likert1value.innerText = likert1.value;
  })
  likert2.addEventListener("change", function(){
    likert2value.innerText = likert2.value;
  })
</script>

 attribute | type  |  |
|--------------|:-------------|:----------------------------------------------------------|
__``start-label``__, __``middle-label``__, __``end-label``__ | ``string`` | Labels for the start, middle and end of the likert scale array.
__``name``__| ``string`` | A name to represent the value generated from the element.
__``value``__| ``number`` | The value that was chosen on the scale.

 event |  |
|--------------|:-------------|:----------------------------------------------------------|
__``change``__ | Dispatched when a selection in the group of choices changes.








## aa-multiple-choice

__``aa-checkboxes``__ implements a single response item, where each available choice is specified as a [``aa-choice-item``](#aa-choice-item).
In the following configuration, the text inside each available choice is used as the value that each `aa-choice-item` corresponds to.


<p class="sticktoexample"></p>

```html
<aa-multiple-choice name="choices">
  <aa-choice-item> choice 1 </aa-choice-item>
  <aa-choice-item> choice 2 </aa-choice-item>
  <aa-choice-item> choice 3 </aa-choice-item>
</aa-multiple-choice>
```

<p class="myexample">
<aa-multiple-choice id="mchoice1">
  <aa-choice-item> choice 1 </aa-choice-item>
  <aa-choice-item> choice 2 </aa-choice-item>
  <aa-choice-item> choice 3 </aa-choice-item>
</aa-multiple-choice>
<br>

<b>current value:</b> <span id="mchoice1value"></span> 
</p>

<script>
  let mchoice1 = document.getElementById("mchoice1");
  let mchoice1value = document.getElementById("mchoice1value");
  mchoice1.addEventListener("change", function(){
    mchoice1value.innerText = mchoice1.value;
  })
</script>


As is the case with [__``aa-checkboxes``__](#aa-checkboxes), ``aa-multiple-choice`` features a ``horizontal`` attribute, and will report ``value`` attributes of ``aa-choice-item`` when specified.

<p class="sticktoexample"></p>

```html
<aa-multiple-choice name="choices" horizontal>
  <aa-choice-item value="1"> choice 1 </aa-choice-item>
  <aa-choice-item value="2"> choice 2 </aa-choice-item>
  <aa-choice-item value="3"> choice 3 </aa-choice-item>
</aa-multiple-choice>
```

<p class="myexample">
<aa-multiple-choice id="mchoice2" horizontal>
  <aa-choice-item value="1"> choice 1 </aa-choice-item>
  <aa-choice-item value="2"> choice 2 </aa-choice-item>
  <aa-choice-item value="3"> choice 3 </aa-choice-item>
</aa-multiple-choice>
<br>

<b>current value:</b> <span id="mchoice2value"></span> 
</p>

<script>
  let mchoice2 = document.getElementById("mchoice2");
  let mchoice2value = document.getElementById("mchoice2value");
  mchoice2.addEventListener("change", function(){
    mchoice2value.innerText = mchoice2.value;
  })
</script>




 attribute | type  |  |
|--------------|:-------------|:----------------------------------------------------------|
__``horizontal``__ | ``boolean`` | Places the available choices in a horizontal layout
__``name``__| ``string`` | A name to represent the value generated from the element.
__``value``__| ``string`` | The value of the item that was chosen.

 event |  |
|--------------|:-------------|:----------------------------------------------------------|
__``change``__ | Dispatched when a selection in the group of choices changes.





## aa-slider

``aa-slider`` implements a Visual Analog Scale (VAS), with labels for minimum and maximum values.

<p class="sticktoexample"></p>

```html
<aa-slider 
  name="slider" 
  min="0"  max="100" 
  min-label="min" max-label="max" 
  value="50">
</aa-slider>
```

<p class="myexample">
<aa-slider id="slider1" min="0" max="100" min-label="min" max-label="max" value="50"></aa-slider>
<br>
<b>current slider value:</b> <span id="slider1value"></span>
</p>

<script>
let slider1 = document.getElementById("slider1");
let slider1value = document.getElementById("slider1value");
slider1.addEventListener("change", function(){
  slider1value.innerText = slider1.value;
})
</script>
 attribute | type  |  |
|--------------|:-------------|:----------------------------------------------------------|
__``min-label``__,  __``max-label``__ | ``string`` | Labels for the minimum and maximum values of the slider.
__``min``__,  __``max``__ | ``number`` | The minimum and maximum numeric values of the range the slider covers.
__``name``__| ``string`` | A name to represent the value generated from the element.
__``value``__| ``number`` | The value that was chosen on the slider.

 event |  |
|--------------|:-------------|:----------------------------------------------------------|
__``change``__ | Dispatched when the value of the slider changes.





## aa-text-answer

<p class="sticktoexample"></p>

```html
<aa-text-answer name="text"></aa-text-answer>
```


<p class="myexample">
<aa-text-answer id="text1"></aa-text-answer>
<br>
<b>current value</b> <span id="text1value"></span>
</p>

<script>
let text1 = document.getElementById("text1");
let text1value = document.getElementById("text1value");
text1.addEventListener("change", function(){
  text1value.innerText = text1.value;
})
</script>

 attribute | type  |  |
|--------------|:-------------|:----------------------------------------------------------|
__``name``__| ``string`` | A name to represent the value generated from the element.
__``value``__| ``string`` | The contents of the textfield

 event |   |
|--------------|:-------------|:----------------------------------------------------------|
__``change``__|  Dispatched when the value of the component changes.







# Putting it all together

## Example of execution

The following example shows how an infinite loop that can be started and ended can be written with the executable HMTL components, and demonstrates the use of [``aa-session``](#aa-session), [``aa-sequence``](#aa-sequence), [``aa-screen``](#aa-screen), [``aa-function-random``](#aa-function-random), [``aa-choose``](#aa-choose), [``aa-when``](#aa-when), [``aa-otherwise``](#aa-otherwise), [``aa-when``](#aa-when)

Pointless as this example may seem, it indicates the freedom the author has to use the custom components within and as containers for HTML, and allow their Ambulator Assessment document to be very customizable.


<p class="sticktoexample"></p>

```html
    <aa-session>
        <template>

            <aa-sequence>
                
                <!-- present an initial screen that prompts starting the process -->
                <aa-screen submit-button-text="next" name="screen1">
                    <p>Press next to run. <br> 
                    You might have to chase after the stop button.</p>        
                </aa-screen>
                
                
                <!-- the second screen contains the loop. 
                Its button will allow us to stop it -->
                <aa-screen submit-button-text="stop" name="screen2">
                    <aa-sequence>

                        <!-- generate an integer value between 1 and 3, 
                        storing it in the variable rnd -->
                        <aa-function-random name="rnd" min="1" max="3"></aa-function-random>

                        <!-- when rnd is 1 insert 😜, 
                             when rnd is 2 insert 🍕, 
                             otherwise insert 🎉 -->
                        <aa-choose>
                            <aa-when test="rnd==1"><span style="font-size:20px">😜</span>​</aa-when>
                            <aa-when test="rnd==2"><span style="font-size:20px">🍕</span><aa-when>
                            <aa-otherwise><span style="font-size:20px">🎉</span></aa-otherwise>
                        </aa-choose>

                        <!-- go back to the point in the inner sequence that generates rnd, 
                        and the sequence will continue from there -->
                        <aa-jump goto="rnd"></aa-jump>
                        <aa-sequence>
                
                </aa-screen>

                <!-- the third and final screen confirms 
                we are at the end of the outer sequence -->
                <aa-screen submit-button-hidden name="screen3"> 
                  <p>The end. Reload the page to restart</h2>
                </aa-screen>

            </aa-sequence>

        </template>
    </aa-session>
```

<p class="myexample">
<aa-session  id="printExampleSession">
<template>
<aa-sequence>
  <!-- present an initial screen that prompts starting the process -->
<aa-screen submit-button-text="next">
<p style="background:none">Press next to run. 
<br>
You might have to chase after the stop button.</p>
</aa-screen>


<!-- the second screen contains the loop. Its button will allow us to stop it -->
<aa-screen submit-button-text="stop">
<aa-sequence>

<!-- generate an integer value between 1 and 3, 
storing it in the variable rnd -->
<aa-function-random name="rnd" min="1" max="3"></aa-function-random>

<!-- when rnd is 1 insert 0, otherwise insert 1 -->

<aa-choose style="font-size:30px">
    <aa-when test="rnd==1"><span style="font-size:20px">&#128540</span></aa-when>
    <aa-when test="rnd==2"><span style="font-size:20px">&#127881</span></aa=when>
    <aa-otherwise><span style="font-size:20px">&#127829</span></aa-otherwise>
</aa-choose>

<!-- go back to the point in the inner sequence that generates rnd, 
and the sequence will continue from there -->
<aa-jump goto="rnd"></aa-jump>
<aa-sequence>
</aa-screen>

<!-- the third and final screen confirms we are at the end of the outer sequence -->
<aa-screen submit-button-hidden> 
<p style="background:none">The end. Reload the page to restart </p>
</aa-screen>
</aa-sequence>
</template>
</aa-session>
</p>









# Composing a questionnaire


The following example demonstrates  a questionnaire that allows participants to report with regard to social encounters.
It consists of the following sections:

## 1. Participant number and gender 

 The participant inputs an identification number (``pNum``) they have been given. They also report their gender. 
 
 This only happens once, subsequent uses of the questionnaire retain the value and don't require it again. So it is only displayed if the participant number is not already known.
 


<p class="sticktoexample"></p>

 ```html
<aa-choose>
  <aa-when test="participantNumber==null">
      <!-- if not, then present a screen that asks for it-->
      <aa-screen name="idScreen" submit-button-text="Next">
          <p>
              Welcome!
              This is the first time you use  this phone to log in.
              <aa-label>Please enter your participant number</aa-label>
              <aa-text-answer type="number" name="pNum" label="participant number">
              </aa-text-answer>
          </p>
          <p>
              <aa-label>Please indicate your gender:</aa-label>
              <aa-multiple-choice name="gender">
                  <aa-choice-item> Male </aa-choice-item>
                  <aa-choice-item> Female </aa-choice-item>
              </aa-multiple-choice>
          </p>
      </aa-screen>
  </aa-when>
  <!-- if participantNumber already had a value,  aa-choose will quietly terminate. -->
</aa-choose>
```
<p class="myexample">
<aa-session><template>      
<aa-choose>
    <aa-when test="1==1">
              
<aa-screen name="idScreen" submit-button-text="Next">
<p>
  Welcome!
  This is the first time you use  this phone to log in.
  <aa-label>Please enter your participant number</aa-label>
  <aa-text-answer type="number" name="pNum" label="participant number">
  </aa-text-answer>
</p>
<p>
  <aa-label>Please indicate your gender:</aa-label>
  <aa-multiple-choice name="gender">
      <aa-choice-item> Male </aa-choice-item>
      <aa-choice-item> Female </aa-choice-item>
  </aa-multiple-choice>
</p>
</aa-screen>
</aa-when>

</aa-choose>
</template></aa-session>
</p>











## 2. Context of interaction 
 
 The second section concerns the context in which the interaction took place. This includes a brief textual description of the interaction (``description``), and questions about its time (``time_of_interacton``),  duration (``length_of_interaction``), location (``place_of_interaction``), medium (``how_of_interaction``), presensce of others (``others_present``) and alcolhol consumption (``drinks``).

<p class="sticktoexample"></p>

```html
<aa-screen name="context" submit-button-text="Next">
    <p>
        Welcome back
        to the social interactions study!
        <aa-label>Brief description of interaction:</aa-label>
        <aa-text-answer name="description" label="description of interaction">
        </aa-text-answer>
    </p>
    <p>
        <aa-label>What time did the interaction start?</aa-label>
        (Report your interactions as soon as possible!)
        <aa-text-answer name="time_of_interaction" type="time" label="time of interaction">
        </aa-text-answer>
    </p>
    <p>
        <aa-label> How long did the interaction last?</aa-label>
        (in minutes)
        <aa-text-answer name="length_of_interaction" type="number"
            label="length of interaction (minutes)">
        </aa-text-answer>
    </p>

    <p>
        <aa-label> Where did the interaction occur?</aa-label>
        <aa-multiple-choice name="place_of_interaction">
            <aa-choice-item value="1"> Student home/own home </aa-choice-item>
            <aa-choice-item value="2"> Family home </aa-choice-item>
            <aa-choice-item value="3"> Work/School </aa-choice-item>
            <aa-choice-item value="4"> Recreation (cafe, cinema, etc) </aa-choice-item>
            <aa-choice-item> Other </aa-choice-item>
        </aa-multiple-choice>
    </p>
    <p>
        <aa-label> How?</aa-label>
        <aa-multiple-choice name="how_of_interaction" single-value>
            <aa-choice-item value="1"> In person (face-to-face) </aa-choice-item>
            <aa-choice-item value="2"> Per telephone </aa-choice-item>
            <aa-choice-item value="3"> Via video (e.g. Skype) </aa-choice-item>
        </aa-multiple-choice>
    </p>

    <p>
        <aa-label> Who was present?</aa-label>
        <aa-multiple-choice name="others_present" single-value>
            <aa-choice-item value="1">One other </aa-choice-item>
            <aa-choice-item value="2"> Multiple people, primary other </aa-choice-item>
            <aa-choice-item value="3"> Multiple people, no primary other </aa-choice-item>
        </aa-multiple-choice>
    </p>

    <p>
        <aa-label> How many alcoholic drinks were consumed?</aa-label>
        (within 3hrs before or during the interaction)
        <aa-text-answer type="number" name="drinks" label="number of drinks">
        </aa-text-answer>
    </p>

</aa-screen>
```

<p class="myexample">
<aa-session><template>
      <aa-screen name="context" submit-button-text="Next">

<p>
    Welcome back
    to the social interactions study!
    <aa-label>Brief description of interaction:</aa-label>
    <aa-text-answer name="description" label="description of interaction">
    </aa-text-answer>
</p>
<p>
    <aa-label>What time did the interaction start?</aa-label>
    (Report your interactions as soon as possible!)
    <aa-text-answer name="time_of_interaction" type="time" label="time of interaction">
    </aa-text-answer>
</p>
<p>
    <aa-label> How long did the interaction last?</aa-label>
    (in minutes)
    <aa-text-answer name="length_of_interaction" type="number"
        label="length of interaction (minutes)">
    </aa-text-answer>
</p>

<p>
    <aa-label> Where did the interaction occur?</aa-label>
    <aa-multiple-choice name="place_of_interaction">
        <aa-choice-item value="1"> Student home/own home </aa-choice-item>
        <aa-choice-item value="2"> Family home </aa-choice-item>
        <aa-choice-item value="3"> Work/School </aa-choice-item>
        <aa-choice-item value="4"> Recreation (cafe, cinema, etc) </aa-choice-item>
        <aa-choice-item> Other </aa-choice-item>
    </aa-multiple-choice>
</p>
<p>
    <aa-label> How?</aa-label>
    <aa-multiple-choice name="how_of_interaction" single-value>
        <aa-choice-item value="1"> In person (face-to-face) </aa-choice-item>
        <aa-choice-item value="2"> Per telephone </aa-choice-item>
        <aa-choice-item value="3"> Via video (e.g. Skype) </aa-choice-item>
    </aa-multiple-choice>
</p>

<p>
    <aa-label> Who was present?</aa-label>
    <aa-multiple-choice name="others_present" single-value>
        <aa-choice-item value="1">One other </aa-choice-item>
        <aa-choice-item value="2"> Multiple people, primary other </aa-choice-item>
        <aa-choice-item value="3"> Multiple people, no primary other </aa-choice-item>
    </aa-multiple-choice>
</p>

<p>
    <aa-label> How many alcoholic drinks were consumed?</aa-label>
    (within 3hrs before or during the interaction)
    <aa-text-answer type="number" name="drinks" label="number of drinks">
    </aa-text-answer>
</p>

</aa-screen>
</template></aa-session>
</p>



## 3. About the primary other.

 If a primary other was present, a subsequennt question asks who it was. If no primary other was present, it does not appear.


<p class="sticktoexample"></p>
```html
<aa-choose>
    <aa-when test="others_present!=3">
        <aa-screen name="partner" submit-button-text="Next" >
            <aa-label> Who was the (primary) other?</aa-label>
            <aa-multiple-choice name="partsex">
                <aa-choice-item>Male</aa-choice-item>
                <aa-choice-item>Female</aa-choice-item>
            </aa-multiple-choice>

            <aa-multiple-choice style="margin-top:40px;" name="relation1">
                <aa-choice-item value="1">Supervisor/teacher</aa-choice-item>
                <aa-choice-item value="2">Co-worker/fellow student</aa-choice-item>
                <aa-choice-item value="3">Supervisee</aa-choice-item>
                <aa-choice-item value="4">Acquaintance</aa-choice-item>
                <aa-choice-item value="5">Friend</aa-choice-item>
                <aa-choice-item value="6">Romantic partner</aa-choice-item>
                <aa-choice-item value="7">Father/mother</aa-choice-item>
                <aa-choice-item value="8">Brother/sister</aa-choice-item>
                <aa-choice-item value="9">Other</aa-choice-item>
            </aa-multiple-choice>
        </aa-screen>
    </aa-when>
</aa-choose>
```

<p class="myexample">
<aa-session><template>
<aa-choose>
<aa-when test="1==1">
<aa-screen name="partner" submit-button-text="Next" autohide=false>
    <aa-label> Who was the (primary) other?</aa-label>
    <aa-multiple-choice name="partsex">
        <aa-choice-item>Male</aa-choice-item>
        <aa-choice-item>Female</aa-choice-item>
    </aa-multiple-choice>

  <aa-multiple-choice style="margin-top:40px;" name="relation1">
      <aa-choice-item value="1">Supervisor/teacher</aa-choice-item>
      <aa-choice-item value="2">Co-worker/fellow student</aa-choice-item>
      <aa-choice-item value="3">Supervisee</aa-choice-item>
      <aa-choice-item value="4">Acquaintance</aa-choice-item>
      <aa-choice-item value="5">Friend</aa-choice-item>
      <aa-choice-item value="6">Romantic partner</aa-choice-item>
      <aa-choice-item value="7">Father/mother</aa-choice-item>
      <aa-choice-item value="8">Brother/sister</aa-choice-item>
      <aa-choice-item value="9">Other</aa-choice-item>
  </aa-multiple-choice>
</aa-screen>
</aa-when>
</aa-choose>
</template>
</aa-session>
</p>






## 5. Behaviours

The protocol calls on the participant to select a number of behaviours they displayed. The available options are grouped in 4 segments, which mix different options together, and only one of those segments is displayed at each sampling session. The choice of the segment to display happens at random.

So we create a variable ``random_choice`` with a random value betwee 1 and 4, and set up an [``aa-choose``](#aa-choose) element with 3 [``aa-when``](#aa-when) children for values or ``random_choice`` between 1 and 3, and an [``aa-otherwise``](#aa-otherwise) child for the remaining value of ``random_choice``
Each of these elements contains a corresponding segment of the list of behaviours, and participants can select all which apply. 


```html
<aa-function-random min="1" max="4" name="random_choice">
</aa-function-random>

<aa-choose>
```

This is the one of three [``aa-when``](#aa-when), for ``random_choice==1`` with its corresponding [``aa-screen``](#aa-screen)

<p class="sticktoexample"></p>

```html
    <aa-when test="random_choice==1">
      <aa-screen name="behaviours1_screen" submit-button-text="Next">
          <aa-label> Did you do any of the following?</aa-label>
          (Multiple answers are possible)

          <aa-checkboxes name="behaviours1">
              <aa-choice-item value="1">
                I listened attentively to the other(s)
              </aa-choice-item>
              <aa-choice-item value="1">
                I tried to get the other(s) to do something else
              </aa-choice-item>
              <aa-choice-item value="1">
                I let other(s) make plans or decisions
              </aa-choice-item>
              <aa-choice-item value="1">
                I expressed the 'real me' instead of my public persona or 'image'
              </aa-choice-item>
              <aa-choice-item value="1">
                I confronted the other(s) about something I did not like
              </aa-choice-item>
              <aa-choice-item value="1">
                I expressed affection with words or gestures
              </aa-choice-item>
              <aa-choice-item value="1">
                I spoke in a clear firm voice
              </aa-choice-item>
              <aa-choice-item value="1">
                I withheld useful information
              </aa-choice-item>
              <aa-choice-item value="1">
                I did not say how I felt
              </aa-choice-item>
              <aa-choice-item value="1">
                I compromised about a decision
              </aa-choice-item>
              <aa-choice-item value="1">
                I took the lead in planning/organizing a project or activity
              </aa-choice-item>
              <aa-choice-item value="1">
                I avoided taking the lead or being responsible
              </aa-choice-item>
              <aa-choice-item value="1">
                I ignored the other(s) comments
              </aa-choice-item>
          </aa-checkboxes>
      </aa-screen>
    </aa-when>
```

<p class="myexample">

<aa-screen name="behaviours1_screen" submit-button-text="Next" autohide=false>
<aa-label> Did you do any of the following?</aa-label>
(Multiple answers are possible)

<aa-checkboxes name="behaviours1">
    <aa-choice-item value="1">
        I listened attentively to the other(s)
    </aa-choice-item>
    <aa-choice-item value="1">
        I tried to get the other(s)
        to do something else
    </aa-choice-item>
    <aa-choice-item value="1">
        I let other(s) make plans or decisions
    </aa-choice-item>
    <aa-choice-item value="1">
        I expressed the 'real me' instead
        of my public persona or 'image'
    </aa-choice-item>
    <aa-choice-item value="1">
        I confronted the other(s) about
        something I did not like
    </aa-choice-item>
    <aa-choice-item value="1">
        I expressed affection
        with words or gestures
    </aa-choice-item>
    <aa-choice-item value="1">
        I spoke in a clear firm voice
    </aa-choice-item>
    <aa-choice-item value="1">
        I withheld useful information
    </aa-choice-item>
    <aa-choice-item value="1">
        I did not say how I felt
    </aa-choice-item>
    <aa-choice-item value="1">
        I compromised about a decision
    </aa-choice-item>
    <aa-choice-item value="1">
        I took the lead in
        planning/organizing a project or activity
    </aa-choice-item>
    <aa-choice-item value="1">
        I avoided taking the lead
        or being responsible
    </aa-choice-item>
    <aa-choice-item value="1">
        I ignored the other(s) comments
    </aa-choice-item>
</aa-checkboxes>
</aa-screen>

</p>


This is the second of three [``aa-when``](#aa-when), for ``random_choice==2`` with its corresponding [``aa-screen``](#aa-screen)



<p class="sticktoexample"></p>

```html      
  <aa-when test="random_choice==2">
      <aa-screen name="behaviours2_screen" submit-button-text="Next" autohide=false>
          <aa-label> Did you do any of the following?</aa-label>
          (Multiple answers are possible)
          <aa-checkboxes name="behaviours2">
              <aa-choice-item value="1">
                  I criticized the other(s)
              </aa-choice-item>
              <aa-choice-item value="1">
                  I smiled and laughed with the other(s)
              </aa-choice-item>
              <aa-choice-item value="1">
                  I spoke softly
              </aa-choice-item>
              <aa-choice-item value="1">
                  I made a sarcastic comment
              </aa-choice-item>
              <aa-choice-item value="1">
                  I expressed an opinion
              </aa-choice-item>
              <aa-choice-item value="1">
                  I complimented or praised the other person(s)
              </aa-choice-item>
              <aa-choice-item value="1">
                  I did not express disagreement when I thought it
              </aa-choice-item>
              <aa-choice-item value="1">
                  I gave incorrect information
              </aa-choice-item>
              <aa-choice-item value="1">
                  I got immediately to the point
              </aa-choice-item>
              <aa-choice-item value="1">
                  I made a concession to avoid unpleaseantness
              </aa-choice-item>
              <aa-choice-item value="1">
                  I did not state my own views</aa-choice-item>
              <aa-choice-item value="1">
                  I expressed the 'real me' instead of my public persona or 'image'
              </aa-choice-item>
          </aa-checkboxes>
      </aa-screen>
  </aa-when>
```

<p class="myexample">

<aa-screen name="behaviours2_screen" submit-button-text="Next" autohide=false>
<aa-label> Did you do any of the following?</aa-label>
(Multiple answers are possible)
<aa-checkboxes name="behaviours2">
    <aa-choice-item value="1">
        I criticized the other(s)
    </aa-choice-item>
    <aa-choice-item value="1">
        I smiled and laughed with the other(s)
    </aa-choice-item>
    <aa-choice-item value="1">
        I spoke softly
    </aa-choice-item>
    <aa-choice-item value="1">
        I made a sarcastic comment
    </aa-choice-item>
    <aa-choice-item value="1">
        I expressed an opinion
    </aa-choice-item>
    <aa-choice-item value="1">
        I complimented or praised the other person(s)
    </aa-choice-item>
    <aa-choice-item value="1">
        I did not express disagreement when I thought it
    </aa-choice-item>
    <aa-choice-item value="1">
        I gave incorrect information
    </aa-choice-item>
    <aa-choice-item value="1">
        I got immediately to the point
    </aa-choice-item>
    <aa-choice-item value="1">
        I made a concession to avoid unpleaseantness
    </aa-choice-item>
    <aa-choice-item value="1">
        I did not state my own views</aa-choice-item>
    <aa-choice-item value="1">
        I expressed the 'real me' instead of my public persona or 'image'
    </aa-choice-item>
</aa-checkboxes>
</aa-screen>

</p>


This is the third of three [``aa-when``](#aa-when), for ``random_choice==2`` with its corresponding [``aa-screen``](#aa-screen)

<p class="sticktoexample"></p>

```html
  <aa-when test="random_choice==3">
      <aa-screen name="behaviours3_screen" submit-button-text="Next">
          <aa-label> Did you do any of the following?</aa-label>
          (Multiple answers are possible)

          <aa-checkboxes name="behaviours3">
              <aa-choice-item value="1">I waited for the other person(s) to talk or act first
              </aa-choice-item>
              <aa-choice-item value="1">I stated strongly that I did not like or that I would not
                  do
                  something</aa-choice-item>
              <aa-choice-item value="1">I assigned someone to a task</aa-choice-item>
              <aa-choice-item value="1">I exchanged pleasantries</aa-choice-item>
              <aa-choice-item value="1">I did not say what was on my mind</aa-choice-item>
              <aa-choice-item value="1">I did not respond to the other(s) questions or comments
              </aa-choice-item>
              <aa-choice-item value="1">I made a suggestion</aa-choice-item>
              <aa-choice-item value="1">I expressed the 'real me' instead of my public persona or
                  'image'</aa-choice-item>
              <aa-choice-item value="1">I showed sympathy</aa-choice-item>
              <aa-choice-item value="1">I did not say what I wanted directly</aa-choice-item>
              <aa-choice-item value="1">I discredited what someone said</aa-choice-item>
              <aa-choice-item value="1">I asked the other(s) to do something</aa-choice-item>
              <aa-choice-item value="1">I spoke favorably of someone who was not present
              </aa-choice-item>
          </aa-checkboxes>
      </aa-screen>
  </aa-when>
```

<p class="myexample">
<aa-screen name="behaviours3_screen" submit-button-text="Next" autohide=false>
  <aa-label> Did you do any of the following?</aa-label>
  (Multiple answers are possible)

  <aa-checkboxes name="behaviours3">
      <aa-choice-item value="1">I waited for the other person(s) to talk or act first
      </aa-choice-item>
      <aa-choice-item value="1">I stated strongly that I did not like or that I would not
          do
          something</aa-choice-item>
      <aa-choice-item value="1">I assigned someone to a task</aa-choice-item>
      <aa-choice-item value="1">I exchanged pleasantries</aa-choice-item>
      <aa-choice-item value="1">I did not say what was on my mind</aa-choice-item>
      <aa-choice-item value="1">I did not respond to the other(s) questions or comments
      </aa-choice-item>
      <aa-choice-item value="1">I made a suggestion</aa-choice-item>
      <aa-choice-item value="1">I expressed the 'real me' instead of my public persona or
          'image'</aa-choice-item>
      <aa-choice-item value="1">I showed sympathy</aa-choice-item>
      <aa-choice-item value="1">I did not say what I wanted directly</aa-choice-item>
      <aa-choice-item value="1">I discredited what someone said</aa-choice-item>
      <aa-choice-item value="1">I asked the other(s) to do something</aa-choice-item>
      <aa-choice-item value="1">I spoke favorably of someone who was not present
      </aa-choice-item>
  </aa-checkboxes>
</aa-screen>
</p>


And the [``aa-otherwise``](#aa-otherwise) child of [``aa-choose``](#aa-choose) contains the last set of behaviours. We could have just as well used an additional ``<aa-when test="random_choice==4">`` instead.
We make sure to also use the closing tag ``</aa-choose>`` at the end.


<p class="sticktoexample"></p>

```html
  <aa-otherwise>

      <aa-screen name="behaviours4_screen" submit-button-text="Next">
          <aa-label>Did you do any of the following?</aa-label>
          (Multiple answers are possible)
          <aa-checkboxes name="behaviours4">
              <aa-choice-item value="1">I expressed the 'real me' instead of my public persona or
                  'image'</aa-choice-item>
              <aa-choice-item value="1">I showed impatience</aa-choice-item>
              <aa-choice-item value="1">I asked for a volunteer</aa-choice-item>
              <aa-choice-item value="1">I went along with the other(s)</aa-choice-item>
              <aa-choice-item value="1">I raised my voice</aa-choice-item>
              <aa-choice-item value="1">I gave information</aa-choice-item>
              <aa-choice-item value="1">I expressed reassurance</aa-choice-item>
              <aa-choice-item value="1">I gave in</aa-choice-item>
              <aa-choice-item value="1">I demanded that the other(s) do what I wanted
              </aa-choice-item>
              <aa-choice-item value="1">I set goals for the other(s) or for us</aa-choice-item>
              <aa-choice-item value="1">I pointed out to the other(s) where there was agreement
              </aa-choice-item>
              <aa-choice-item value="1">I spoke only when I was spoken to</aa-choice-item>
              </aa-choice-item>
          </aa-checkboxes>
      </aa-screen>
  </aa-otherwise>


</aa-choose>
```

<p class="myexample">

<aa-screen name="behaviours4_screen" submit-button-text="Next" autohide=false>
          <aa-label>Did you do any of the following?</aa-label>
          (Multiple answers are possible)
          <aa-checkboxes name="behaviours4">
              <aa-choice-item value="1">I expressed the 'real me' instead of my public persona or
                  'image'</aa-choice-item>
              <aa-choice-item value="1">I showed impatience</aa-choice-item>
              <aa-choice-item value="1">I asked for a volunteer</aa-choice-item>
              <aa-choice-item value="1">I went along with the other(s)</aa-choice-item>
              <aa-choice-item value="1">I raised my voice</aa-choice-item>
              <aa-choice-item value="1">I gave information</aa-choice-item>
              <aa-choice-item value="1">I expressed reassurance</aa-choice-item>
              <aa-choice-item value="1">I gave in</aa-choice-item>
              <aa-choice-item value="1">I demanded that the other(s) do what I wanted
              </aa-choice-item>
              <aa-choice-item value="1">I set goals for the other(s) or for us</aa-choice-item>
              <aa-choice-item value="1">I pointed out to the other(s) where there was agreement
              </aa-choice-item>
              <aa-choice-item value="1">I spoke only when I was spoken to</aa-choice-item>
              </aa-choice-item>
          </aa-checkboxes>
      </aa-screen>
</p>


## 6. Feelings

The next part of the questionnaire asks participants to rate


 <aa-likert-scale name="surprised" items="12" start-item="0"></aa-likert-scale>



```


    <aa-screen name="feelings">

        <div>
            <aa-label> How did you feel?</aa-label>
            (0 = Not at all, 6 = Extremely)
        </div>


        <aa-label> Surprised</aa-label>
        <aa-likert-scale name="surprised" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Worried/anxious</aa-label>
        <aa-likert-scale name="worried" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Happy</aa-label>
        <aa-likert-scale name="happy" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Frustrated</aa-label>
        <aa-likert-scale name="frustrated" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Pleased</aa-label>
        <aa-likert-scale name="pleased" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Angry/hostile</aa-label>
        <aa-likert-scale name="angry" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Homesick</aa-label>
        <aa-likert-scale name="homesick" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Enjoyment/fun</aa-label>
        <aa-likert-scale name="enjoyment" items="7" start-item="0"></aa-likert-scale>


        <aa-label> Joyful</aa-label>
        <aa-likert-scale name="joyful" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Depressed/blue</aa-label>
        <aa-likert-scale name="depressed" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Energized </aa-label>
        <aa-likert-scale name="energized" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Ashamed </aa-label>
        <aa-likert-scale name="ashamed" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Safe</aa-label>
        <aa-likert-scale name="Safe" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Lonely</aa-label>
        <aa-likert-scale name="lonely" items="7" start-item="0"></aa-likert-scale>

        <aa-label> Disgusted by the other(s)</aa-label>
        <aa-likert-scale name="disgusted" items="7" start-item="0"></aa-likert-scale>

    </aa-screen>

    <aa-choose>
        <aa-when test="others_present!=3">
            <aa-screen name="perceptions">
                <div>
                    <aa-label> Indicate how the (primary) other behaved towards you in this interaction
                    </aa-label>
                </div>
                <aa-affect-grid style="margin-top:40px" top-left-label="Critical"
                    top-label="Assured/Dominant" top-right-label="Engaging"
                    left-label="Cold/Quarrelsome" right-label="Warm/Aggreeable"
                    bottom-left-label="Withdrawn" bottom-label="Unassured/Submissive"
                    bottom-right-label="Deferring" rows=11 columns=11>
                </aa-affect-grid>
            </aa-screen>
        </aa-when>
    </aa-choose>


    <aa-screen submit-button-hidden="true" name="ending">
        <h3>Thank you!</h3>
        You can now close this window.
    </aa-screen>

</aa-sequence>

</template></aa-session>

```