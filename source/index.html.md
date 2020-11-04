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

```html
Wikipedia is at www.wikipedia.org

Wikipedia is <a href="http://www.wikipedia.org">here</a>
```


The first line contains a simple mention of a url. The second one contains themarkup ``<a href="...">here</a>`` that tells the computer to display the content( the word ``here`` ) of that markup (``<a>``) as a clickable link, that links to the url contained in the attribute ``href``.  ``a`` stands for Anchor. An anchor is a piece of text which marks the beginning and/or the end of a hypertext link.

The lines above are each displayed as:

<p class="myexample">
  Wikipedia is at www.wikipedia.org
  <br><br>
  Wikipedia is  <a href="www.wikipedia.org">here</a>
</p>


## Tags, Attributes, Elements

In HTML, the annotation syntax is called __HTML tag__, which consists of two parts; an __*opening tag*__, e.g., __``<a>``__  and a corresponding __*closing tag*__, e.g, __``</a>``__. The part of the text that consists of an opening tag, its corresponding closing tag, and the contents inbetween is called an __HTML element__. The HTML tag marks the start and end of the HTML element. The text inbetween  the start and end tag is the content. As such, sometimes an element is referred to as a container.

An opening tag can also contain __attributes__. Attributes further define details about the element, e.g. in ``<a href="htttp://www.wikipedia.org>here</a>``, the attribute ``href`` defines the url, which this anchor element links to.



## Parents and Children


<!-- #### Further examples


__Paragraph ``<p>`` :  ``<> -->


# Executable HTML

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

This example illustrates triggering a sequence through a button. We listen for the ``sessionReady`` event to make sure ``#button1 has been inserted in the document, and set a click event listener, that calls the ``.next()`` member function of ``#sequence1``. Pressing the next" button adds content as per the sequence.


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

[``aa-screen``](#aa-screen), discussed below, is an element that simplifies managing ``aa-sequence`` in user interactions.


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

``aa-likert-scale`` is a preconfigured [``aa-multiple-choice``](#aa-multiple-choice) item that implements a 5-item or 7-item rating scale. It also supports labels at start, middle and end.

```html
<aa-label>scale 1:</aa-label>
<aa-likert-scale name="my5itemScale" items="5"
  start-label="start" 
  middle-label="middle" 
  end-label="end" 
  ></aa-likert-scale>

<aa-label>scale 2:</aa-label>
<aa-likert-scale items="my7itemScale"></aa-likert-scale>
```
<p class="myexample" id="aaLikertScaleContainer">
<aa-label>scale1:</aa-label>
  <aa-likert-scale id="likert1" name="my5itemScale" start-label="start" middle-label="middle" end-label="end" items="5"></aa-likert-scale>
  <br>
  <aa-label>scale 2:</aa-label>
  <aa-likert-scale id="likert2" items="7"></aa-likert-scale>

  <br>
  <b>value of scale 1:</b> <span id="likert1value"></span>
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

```html
<aa-slider name="slider" min="0" max="100" min-label="min" max-label="max" value="50" ></aa-slider>
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




















