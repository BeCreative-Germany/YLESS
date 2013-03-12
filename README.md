#YLESS
YLESS (experimental) is a new concept to extends CSS with dynamic behavior such as variables, mixins, operations and 
functions it is very simple,lightweight and fast. YAML is a human-readable data serialization format it is a 
superset of JSON and is very clear and comfortable. 
Why we dont´t use a format which we already know and every day use? It use YAML as alternative for the CSS Markup language and Javascript for function execution and calculation.

I´m not very experience developer in parsing or compiling. My idea behind this project is why we take the 
trouble to create new languages to extend the css behaviour like 'simpleLESS' when we have already all present.

YLESS use the YAML-format it is very easy to read and to extend and the big factor for me is that YAML is not a nobody in the world
of programming so i think lots of people will like it. You can write your own template function in pure Javascript and all variables or function
are available:


####Access of template variables in functions
`this.<variableName>`
####Access of template functions in functions
`this.functions.<functionsName>`

####Access of template variables
`$<variableName>`
####Access of template functions
`<functionName>(args ...)`
Notice: YLESS allows only the use of one function for one selector property (strong style guideline)


##Features:
+ Variables
+ Mixins (property inheritance)
+ Template functions (Javascript)
+ Variable calculation

###TODO
+ Error handling
+ some improvements

Notice: This project from me is very new and it has currently no good status for publishing.

###Units (px|em|cm|in|mm|ex|pt|pc|%)
The parser works from left to right so the first unit which is found is used for the whole caculation.

###Big thanks to the contributors of these awesome opensource software:
+ js-yaml [nodeca](https://github.com/nodeca/)
+ commander.js [visionmedia](https://github.com/visionmedia/)

##Option parsing
```

  Usage: yless.js [options] -i <input> -o <output>

  Options:

    -h, --help          output usage information
    -V, --version       output the version number
    -i, --ifile <path>  Input YAML file
    -o, --ofile <path>  Output CSS file
    -c, --compress      Compress CSS


```
####Example
`./yless.js -i "yless/test.yless" -o "css/test.css"`
##Example (test.yless):
```YAML
#YLESS Translator

# Translator Information
Translator:
  Author : Dustin Deus
  Version  : "0.0.1"

#Declare custom variables
Runtime:
  variables:
    color :  "#DEDEDE"
    width :  150.50px + 100px
    height : (250px + 3) + 1
    radius:  6px

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
height:254px;
max-height:510px;
max-width:594px;
padding:3px 4px 2px 1px;
font-weight:bold;
font-size:4em;
background-color:red;
}
.redBackground{
background-color:red;
border-radius:6px;
-moz-border-radius:6px;
-webkit-border-radius:6px;
}
.rounded-borders{
border-radius:6px;
-moz-border-radius:6px;
-webkit-border-radius:6px;
}
div#foobar {
background-color: #FFFFF;
width: 250.5px;
}
```
##Compress (with -c parsing option)
```CSS
div#test li.base{font-weight:bold;font-size:4em;}div#test li.sub{color:#DEDEDE;width:250.5px;height:254px;max-height:510px;max-width:594px;padding:3px 4px 2px 1px;font-weight:bold;font-size:4em;background-color:red;}.redBackground{background-color:red;border-radius:6px;-moz-border-radius:6px;-webkit-border-radius:6px;}.rounded-borders{border-radius:6px;-moz-border-radius:6px;-webkit-border-radius:6px;}div#foobar li {background-color: #FFFFF;width: 250.5px;}
```
##License
(The MIT License)

Copyright (C) 2013 by Dustin Deus

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
