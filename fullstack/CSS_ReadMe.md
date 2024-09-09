																		CSS

Syntax :
	selector {declaration}

	- tag
		direct  
		p {}
	- id 
		#
		#header {}

	- class 
		.
		.comment {}

Adding CSS:
	- inline
		inside tag  		=> for perticular element
	- embedded
		inside head tag   	=> for perticular page
	- external
		in another file	  	=> for all pages

Comments :
	/*  */

Dif b/w class and id 
	class : we can use multiple times on a page
	id : unique on a page

CSS conflicts and cascade:
	if identical then 
		- top to bottom  ( low to high )
		- external to inline ( low to high )
Inheritance :
	Nearest parent properties will have high priority
	
Specificity :
	- order not considered
	Id 100 > Class 10 > Element 1

Always go with Inheritance then Specificity ( high to low )

Important Declaration :
	selector : { declar  ation ! important }

Targeting multiple elements :
	separated by comma

Descendant Selector :
	Parent to Child
	#Id1 #Id2 .Class1 .Class2 Element1 Element2

Combine Selectors: 
	and Id selector :
		selector1#Id selector2
	and Class selector :
		selector1.class selector2

Def btw #id selector and  selector#id
		#id selector   => it will point out the selector which is inside the block which is equal to id
		selector#id   => if will point out the selector which is equal to id
		
Child Selecotr : 
	selects direct children
	#id > element

Adjacent Selectors :
	select element after an element
	#Id selecor1 + selector2
 
Attribute Selector :
	- element[attr]
	- element[attr = value]  => if attr.value == value
	- element[attr ~= value] => value in attr.value.split()
	- element[attr $= value] => attr ends with value		$
	- element[attr ^= value] => attr starts with value

Pseudo Selecotrs :
	Dynamic :
		- based on user action
		- #Id:hover{}     ( active , visited )
	Structural :
		- advanced structural
		
		first and last child:
			selector : first-child
			selector : last-child     => selector should be the first child then only it will select (like > ) .

		only-child : 
			selector1 selector2 : only-child   => selects only slct2 from slct1

		first and last of type selector:
			slct1 slct2 : first-of-type
			slct1 slct2 : last-of-type

		only-of-type :
			slct1 slct2:only-of-type

		nth-child:
			selector : nth-child(1) , nth-child(2) , nth-child(even) , nth-child(odd) , nth-child(2n + 1)

		nth-last-child :
			slct : nth-last-child(2)   => counting from back

		nth-of-type :
			selector : nth-of-type(1) , nth-of-type(5) , nth-of-type(even) , nth-of-type(odd) , nth-of-type(2n + 1)

		empty :
			slct : empty 

		Negation :
			slct:not(#id , .class , :first-child)

Universal Selector :
	* { Declaration }

General Sibling Selector :
	A ~ B  => selects all B followed by A

font-size :
	based on inheritance => overriding the actual size 
	16px  ,  4em  (4 times of actual) , 50%

font-family :
	separated by comma if fails
 
 text-decoration :
 	inherit , underline ,overline

 font-weight :
 	ligher , bolder , 100-500-900

 text-transform :
 	capitalize , lowercase , uppercase

 colors :
 	color , backgroundcolor

styling links
letter-spacing , word-spacing , line-height , margin-bottom 

FLEXBOX FROGGY :
	justify-content (Horizontally): flex-end , flex-start , center , space-between , space-around
	align-items  	(Vertically)  :	flex-end , flex-start , center , baseline , stretch
	align-self		(  ||      )  : Same but only for perticular item
	align-content				  : irrespective of distance between items

	flex-direction 				  : row (items placed left to right), row-reversed , column (items placed top to bottom) , column-reversed 
	flex-wrap 					  : nowrap , wrap , wrap-reverse

	flex-flow =>  flex-direction + flex-wrap : takes two inputs (direction , wrap)

	If we change the direction from row to column then for horizontal -> align-items and for vertical -> justify-content will be used.

	Order :
		order : integer   => starts from 0 , -1 means before all


Resources:
	https://www.youtube.com/watch?v=I9XRrlOOazo&list=PL4cUxeGkcC9gQeDH6xYhmO-db2mhoTSrT
	https://flexboxfroggy.com/
