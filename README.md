### There is 4 lifetime of variable
1. Birth - variable initialized in function
2. Death - variable is cleaned in function
3. Local - variable doesn't leave a function
4. Wide - variable comes from another function
```G
{
	// Birth variable, local one
	// (int a)()()
	int a

	// it can have wide lifetime
	// ()print(int a)()
	print(a)
	// Death variable due being at the end of the scope
	// ()()(int a)
}
```
### Function anatomy
1. First brackets - *birth* variables
2. Function name - name of function
3. Second brackets - *wide* variables
4. Third brackets - *death* variables
```G
// Calling a function First and Third brackets are optional when null
() = print(variable)()
print(variable)()
() = print(variable)
print(variable)

// When using a constructor to initialize a variable Second and Third brackets are optional when null
int a
int a = ()
int a = ()()

// In constructor when third brackets isn't null then second brackets are expected
// Wrong!
// int a = (2)
int a = ()(2)
```
### Constructors
1. Functions without names
2. Triggers when conditions are met
3. Base constructors are available like assign value from one variable to another when type is complete match
4. Constructors doesn't trigger another constructor until specific constructor is called (May cause memory leak when variable is declared inside constructor isn't managed in constructor)
```G
int a

// Triggers when variable is declared
(*int this)()() {
	(<int>this)()()
}

(*[]char string)(*int this)() {
	// convert number to string representation
}

()(*int this)(int value) {
	<int>this = value
}

// Triggers at the end of the scope or when it passed to Third bracket after child function finished execution
()()(*int this) {
	()()(<int>this)
}
```
### Memory structure
Memory tree ends when either 2 conditions are met
1. It hits the end (leaf)
2. It hits a pointer to another memory structure
Third bracket deletes a memory structure. 
basic blocks of variable
- simple types which used in C like int, char etc
- pointer (must know a memory structure which points)
- \[7] - shows that there is 7 elements of sub-memory structure which is located close to one another. Exception: when pointer points to array then size of array is blank.
- {} - shows that there are sub-memory structure of different memory size
- struct - keyword to declare a name for memory structure. Must begin with capital letter!
```G
// Memory block: int
int a
// Memory block: *
*int b
// Memory block: [3]int
[3]int c
// Memory block: *
*[]char d
// Memory block: {*, int}
{*[]char string, int length} e

// struct {name} {variable type}
struct Hello int
// Memory block: int
Hello f
```
All variables are public.
### Templates
When variable is unknown then Template are used. 
```
// Memory block: {*, int}
struct SmartPointer<T> {
	*T value,
	int count
}

(SmartPointer<T> this)()() {
	this.count = 0
	this.value = NULL
}

(*SmartPointer<T> this)()() {
	(<SmartPointer<T>>this)()()
}

(*SmartPointer<T> this)()(T value) {
	(this)()()
	(this.<T>value)()()
	this.<T>value = value
}

(*SmartPointer<T> this)(*SmartPointer<T> that)() {
	this = that
	this.count++
}

()()(SmartPointer<T>) {
	this.count--
	if (this.count == 0) {
		()()(this.<T>count)
	}
}

()()(*SmartPointer<T>) {
	()()(<SmartPointer<T>>this)
}

SmartPointer<[6]char> string = ()("Hello")
```
### Type cast
Useful in OOP. Examples shown as (variable type) (variable name)
- <(type)>(variable name) - cast variable name's type to specific type
- <(parent type)>(variable name) - all children of parent type have an access to use that varialbe
- (variable name)\<type> - used template for specific memory block.
