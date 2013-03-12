#YLESS
YLESS (experimental) is a new concept to extends CSS with dynamic behavior such as variables, mixins, operations and 
functions it is very lightweight and fast. YAML is a human-readable data serialization format it is a 
superset of JSON and is very clear and comfortable. 
Why we dont´t use a format which we already know and every day use? It use Javascript for function execution and calculation.

I´m not very experience developer in parsing or compiling. My idea behind this project is that why we take the 
trouble to create languages to extend the css behaviour like 'simpleLESS' when we have already all present.

YLESS use the YAML-format it is very easy to read and to extend and the big factor for me is that YAML is not a nobody in the world
of programming so i think lots of people will like it. You can write your own template function in pure Javascript and all variables or function
are available:


####Access of template variables
`this.<variableName>`
####Access of template functions in functions
`this.functions.<functionsName>`


###Features:
+ Variables
+ Mixins (property inheritance)
+ Template functions (Javascript)
+ Variable calculation

Notice: This project from me is very new and it has currently no good status for publishing.

###Use of opensource:
+ Big thanks to nodeca [js-yaml](https://github.com/puzrin)

##Example (test.yless):
```YAML
#YLESS Translator

# Translator Information
Translator:
  Author : Dustin Deus
  Version  : "1.0"

#Declare custom variables
Runtime:
  variables:
    color :  "#DEDEDE"
    width :  150.50px + 100px
    height : (250px + 3) + 1
    radius:  5px

#############Selectors##############
Selectors:
  #selector 1
  div#test li.base:
    font-weight : bold
    font-size : 3em + 1em

  #selector 2
  div#test li.sub:
    color : "#DEDEDE"
    width : $width #Access of variables in selectors
    height : $height
    max-height : (20 + 150px ) * 3
    max-width : maxWidth(54,33,$width + 2*(13-10)) #Use Javascript function for calculations
    padding: 3px 4px 2px 1px
    $extends : 'div#test li.base, .redBackground'  #(mixins)

  #selector 3
  .redBackground:
    background-color: red
    $extends : '.rounded-borders'

  .rounded-borders:
    border-radius: $radius
    -moz-border-radius: $radius
    -webkit-border-radius: $radius

Functions:
  #Declare custom functions
  maxWidth: !!js/function >
    function (v,c,n) {
      return this.width + v + c + n; //Access of variables in Javascript
    }

#For raw CSS with only variables 
RawCSS: |
  div#foobar {
  background-color: #FFFFF;
  width: $width;
  }

```
##Output:

```CSS
div#test li.base{
font-weight:bold;
font-size:4em;
}
div#test li.sub{
color:#DEDEDE;
width:250.5px;
height:250px;
max-height:510px;
max-width:587.5px;
padding:3px 4px 2px 1px;
font-weight:bold;
font-size:4em;
background-color:red;
}
.redBackground{
background-color:red;
border-radius:5px;
-moz-border-radius:5px;
-webkit-border-radius:5px;
}
.rounded-borders{
border-radius:5px;
-moz-border-radius:5px;
-webkit-border-radius:5px;
}
div#foobar {
background-color: #FFFFF;
width: 250.5px;
}
```

###TODO:
+ Error reporting 
+ Function handling
+ Unit convertion
