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
    height : 250px
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
    max-width : maxWidth(54,33,$width) #Use Javascript function for calculations
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
