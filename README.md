![YLESS](http://starp-germany.de/blog/wp-content/uploads/2013/03/yless1.jpg)

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
`this.call(<functionsName>,<Args>)`

####Use of template variables
`$<variableName>`
####Use of template functions as property value
`<functionName>(args ...)`
Notice: YLESS allows only the use of one function for one selector property (strong style guideline)


##Features:
- [x] Variables
- [x] Mixins (property inheritance)
- [x] Template functions (Javascript)
- [x] Variable calculation

##TODO
- [ ] File importing
- [ ] Better error reporting
- [ ] Multiple file parsing

##Error reporting
By the reason of the simpleness of the translator errors are hard to report becouse the translator doesn´t parse any value but only
function calls and calculations. Follwing error reports are supported:
- Function existence
- Right count of functions arguments
- Extends selectors existence
- Variable existence

##Event-base debugging mode (parsing option -d)
```CSS
Event: extendSelector with:  div#test li.sub extends by div#test li.base
Event: extendSelector with:  div#test li.sub extends by .redBackground
Event: extendSelector with:  .redBackground extends by .rounded-borders
Event: recognizeEquation with:  150.50em + 100px
Event: evalEquation with:  150.50 + 100
Event: recognizeEquation with:  1*(250px + 3) + 1
Event: evalEquation with:  1*(250 + 3) + 1
Event: replaceVariableRaw with:  $width replaced by "250.5em"
Event: recognizeEquation with:  3em + 1em
Event: evalEquation with:  3 + 1
Event: replaceVariable with:  $width replaced by "250.5em"
Event: startFuncArguments with:  [ '' ]
Event: functionCall with: Name: maxWidth , Arguments: 1,2,3
Event: recognizeEquation with:  (20 + 150px ) * 3
Event: evalEquation with:  (20 + 150 ) * 3
Event: replaceVariable with:  $width replaced by "250.5em"
Event: startFuncArguments with:  [ '54', '33', '250.5em + 2*(13-10)' ]
Event: evalEquation with:  250.5 + 2*(13-10)
Event: afterExecFuncArguments with:  [ 54, 33, 256.5 ]
Event: recognizeEquation with:  3em + 1em
Event: evalEquation with:  3 + 1
Event: replaceVariable with:  $radius replaced by "5px"
Event: replaceVariable with:  $radius replaced by "5px"
Event: replaceVariable with:  $radius replaced by "5px"
Event: replaceVariable with:  $radius replaced by "5px"
Event: replaceVariable with:  $radius replaced by "5px"
Event: replaceVariable with:  $radius replaced by "5px"
```

Notice: This project from me is very new and it has currently no good status for publishing.

###Units (px|em|cm|in|mm|ex|pt|pc|%)
The parser works from left to right so the first unit which is found is used for the result.

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
    -d, --debug         Debug-mode

```
####Example
`./yless.js -i "yless/test.yless" -o "css/test.css"`
##Example CSS in YAML:
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
    width :  150.50em + 100px
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
    height : foobar()
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
  foobar: !!js/function >
    function () {
      return this.call('maxWidth',1,2,3);
    }

#For raw CSS with only variables 
RawCSS: |
  div#foobar li {
  background-color: #FFFFF;
  width: $width;
  }
```
##Output by YLESS:

```CSS
div#test li.base{
font-weight:bold;
font-size:4em;
}
div#test li.sub{
color:#DEDEDE;
width:250.5em;
height:256.5;
max-height:510px;
max-width:594em;
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
div#foobar li {
background-color: #FFFFF;
width: 250.5em;
}

```
##Compress (with -c parsing option)
```CSS
div#test li.base{font-weight:bold;font-size:4em;}div#test li.sub{color:#DEDEDE;width:250.5em;height:256.5;max-height:510px;max-width:594em;padding:3px 4px 2px 1px;font-weight:bold;font-size:4em;background-color:red;}.redBackground{background-color:red;border-radius:5px;-moz-border-radius:5px;-webkit-border-radius:5px;}.rounded-borders{border-radius:5px;-moz-border-radius:5px;-webkit-border-radius:5px;}div#foobar li {background-color: #FFFFF;width: 250.5em;}
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
