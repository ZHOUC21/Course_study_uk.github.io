# Objected Oriented Programming in Java                  
So far our recipe for a class can be shown below, so far our framework has class(or static) methods and class(or static) variables. Subsequently we would introduce the instance variables and instance methods.                
```java
class Classname {
	static <type> variable;
	
	static <type> method1 (<parameter_list>) {
	// ...
	}
	
	public static void main (String[] args) {
	// ...
	// where we would often use the method to transform the input string variables into integer or float types:
	// From string to integer:
	a = Integer.parseInt(args[0]);
 	
	// From string to double:
	a = Double.parseDouble(args[0]);
	
	// From int/dooube to string:
	args[0] = String.valueOf(a);
	args[1] = Integer.toString(b);
	}
}
```
where the static method parseInt of the class Integer has been called by stating **a = Integer.parseInt(args[0]);**.              


          
                    
**_Yours,_**                         
**_Chuwei Zhou_**                 
**_2018.11.10_**                     
 

