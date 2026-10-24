## what is wrapper
A wrapper class in Java is a class whose object wraps or contains a primitive data type. It provides a way to use primitive data types (int, char, double, etc.) as objects.
Java is an object-oriented language, but primitives are not objects. Wrapper classes bridge this gap.

The Wrapper Class Mapping
Each of the eight primitive types has a corresponding wrapper class located in the java.lang package:

Primitive Type	Wrapper Class
int	Integer [1]
char	Character [1]
double	Double [1]
boolean	Boolean [1]
byte	Byte [1]
short	Short [1]
long	Long [1]
float	Float [1]


Why Do We Need Wrapper Classes?

• Java Collections: Frameworks like ArrayList, HashSet, or HashMap cannot store primitives. They require objects.java
// Invalid: ArrayList<int> list = new ArrayList<>();
ArrayList<Integer> list = new ArrayList<>(); // Valid


• Utility Methods: They provide built-in methods for conversion, parsing, and data manipulation (like Integer.parseInt() or Character.isDigit()).

• Null Values: Primitives must always have a value (e.g., int defaults to 0). Wrapper objects can be null, which is useful in databases to represent a missing value.

Autoboxing and Unboxing
Java automatically converts between primitives and their wrapper objects behind the scenes:

• Autoboxing: Automatic conversion of a primitive type into its corresponding wrapper class object.java
int num = 5;
Integer wrappedNum = num; // Autoboxing


• Unboxing: Automatic conversion of a wrapper class object back into its corresponding primitive type.java
Integer wrappedNum = 10;
int num = wrappedNum; // Unboxing