# Universal Coroutine Based Lazy Sequence Manipulation
>  A description of a universal implementation of lazy sequence manipulation via coroutines (or generators)



This repository proposes a universal implementation description of lazy sequence manipulation using coroutines/generators in any programming language that supports them.



## Introduction

This description will use a custom pseudocode language that is easy to understand and will avoid to tie the description to any programing language. The language may share similarities with C++, JavaScript and PHP.

It will also favor a statically typed description so that it may be implemented in statically typed languages without any hassle.

It will propose an object oriented representation, it is up to you to adapt it to how the target language works.



## Inspirations

* C++'s [Range V3](https://github.com/ericniebler/range-v3)
* JavaScript's [sequency](https://github.com/winterbe/sequency)
* Java 8's [Stream API](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
* Kotlin's [sequences](https://kotlinlang.org/docs/sequences.html)
* Rust's [iter](https://doc.rust-lang.org/std/iter/index.html#laziness)



## Terminology

A **Stream** is a (maybe infinite) sequence of data of one particular type (e.g. `Stream<int>`). It may hold the minimum amount of state to be evaluated. A *Stream* must have a **parent** *Stream* or *Coroutine* from which it gets data.

An **Operation** is a [morphism](https://en.wikipedia.org/wiki/Morphism) that transforms values of one type into values of another type (which may be the same).

A **Creator** is an *Operation* that transforms data that is not a *Stream* into a *Stream*. For instance: `streamFromArray([])`. 

An **Operator** is an *Operation* that transforms a *Stream* of a given type into a *Stream* of another type (which may be the same). For instance: `stream.map(identity)`. These operations are lazy evaluated, meaning they are not evaluated until a *Terminator* is applied. An *Operator* may be *Stateful* or *Stateless*. These operations are lazy evaluated, meaning they are not evaluated until a *Terminator* is applied.

**Internal State** designates state that is not directly derived from the input of the *Operation*. For instance: a set a already processed values.

A **Stateful Operator** is an *Operator* that needs to maintain *Internal State* for its evaluation. For instance `stream.unique()` or `stream.take(5)`. If it does not introduce a change in complexity, then it can be considered *Stateless*.

A **Stateless Operator** is an *Operator* that does not need to maintain internal state for its evaluation. For instance `stream.filter(predicate)`.

A **Terminator** is an *Operation* that transforms a *Stream* of a given type into values of data of another type (which may be the same but must not be a *Stream*). For instance: `stream.forEach(print)`.

A **Coroutine** or **Generator** describes a mechanism that allows to "lazily" provide values *one at a time*. For the sake of implementation, they must be monadic: a *Coroutine* must be able to provide the values of another *Coroutine*; put simple they must be *Iterable*.

An **Iterable** provides an *Iterator* to access a sequence of data.

An **Iterator** is a view-like mechanism to traverse a sequence of data that holds info like its validity (are there more?) and the current value.

A **Coroutine/Generator factory** is a special kind of function which, once called, returns a *Coroutine/Generator*.

A **Sequence Manipulation** is a sequence of *Operation*s that start with one *Creator*, may contain several *Operator*s, and ends with a *Terminator*.

An **Evaluation** is the process of evaluating an *Operation*, i.e. giving it its input and getting its output.



## Complexity

A *Sequence manipulation* that only uses *Stateless Operator*s is guaranteed to be ***O(n)*** which means it is strictly equivalent to a single for loop.

A *Sequence Manipulation* that uses **k** *Stateful Operators* is guaranteed to be at worst ***O(n^(k+1))*** which means it is equivalent of **k+1** nested for loops.



## Pseudocode language details

This language does not take into account memory management, lifetime and so on. It is your duty to make the right choices in your implementation.



`template <T>` provides the type `T` as a type argument for generic implementations of classes or functions.



`@StaticMethod` denotes that the decorated function may be used as a static method of the return type. As such, the decorated function cannot be used as a free function unless it is also decorated with `@FreeFunction`. They also allow the use of the `static` keyword to refer to the type for which they are a static method. For instance:

```
template <T>
@StaticMethod
MyType<T> stuff(){
	// return MyType<T>{};
	return static{};
}

MyType::stuff<int>();
MyType<int>::stuff();
```



`@FreeFunction` denotes that the decorated function may be used as a free function.



`@ExtensionMethod` denotes that the decorated function is an (extension) method of the type of the `this` argument of the function which must be present and must be the first. As such, the decorated function cannot be used as a free function. As they are extensions, they are public and also have access to private and protected members. Like static methods, they have access to the `static` keyword to refer to the current type.

```
@ExtensionMethod
int zero(int[] this){
	return 0;
}

[1,2,3].zero() == 0;
```



For simplicity, the `auto` type may be used, it behaves exactly as the C++ keyword.



Anonymous functions are literal functions that may be assigned to variables. They do not use external state unless explicitly specified. Note that lambda functions are a variation of anonymous functions that cannot contain a body (i.e. it directly returns a single expression) and automatically "capture" external state:

```
auto fn = function(x){ return x * 2; };
auto fn = x => x * 2;

auto y = 32;
auto life = function(x = 10) using(y) { return x + y };
auto life = (x = 10) => x + y;
```





*Coroutine/Generator factories* are declared as special anonymous functions that can yield data:

```
auto factory = function* (my_args){
	yield stuff; // yield a single value
	yield* [1,2,3];
	// yield all from an iterable, as if using a for loop and yielding each element individually
};

auto gen = factory(args);
```



Callable types may have contravariance/covariance:

```
template <Arg, R>
type MyCallable<Arg, R> = (Arg) => R;

MyCallable<String, int> a = str => str.length;
MyCallable<String, int> b = () => 0; // also valid, just ignores the argument

a("str");
b("str");
b(); // invalid as it requires an argument even if the implementation doesn't

template <T>
type MyCallback<T> = (T) => void;

MyCallback<String> a = function(String str){
	print(str);
};

MyCallback<String> b = str => str; // also valid, just ignores the return value
```





Classes encapsulate data with behavior. By default members are public, unless an encapsulation guard is specified. Classes may implement/extend other classes using the `:` notation (as in C++). The use of the `this` keyword is mandatory. Classes may only have one constructor (to allow for maximum interoperability). The `static` keyword may be used to refer to the current type for inheritance.

```
class MyClass : MyBaseClass, MyInterface, MyCrtp<static> {
	constructor(int stuff){
		this.stuff = 0;
	}
	
	protected:
		int stuff;
		
	public:
		int getStuff(){ return this.stuff; }
		
	private:
		float stuff2 = 42.05;
}
```



Objects are instantiated using C++'s uniform initialization syntax: `MyClass obj{}` or `MyClass obj = {}` or `auto obj = MyClass{}`. Methods on objects are accessed using dot notation: `obj.method()`. Methods on classes/types are accessed using namespace notation: `MyType::myStaticMethod()`.



Classes may nest type alias declarations:

```
class MyClass {
	type MyAlias = int;
	
	template <U>
	type MyArray<U> = U[];
}
```



Type aliases may also be global. There is a special notation to declare callable type aliases:

```
type MyGlobalAlias = String;

template <T>
type MyCallable<T> = (int) => T; // anonymous function, lambda, coroutine factory, callable object, etc...
```



This language also uses several types for simplicity:

* `Optional<T>` which is either constructed from `some(T)` or `none`
* `Either<L, R>` which is either `L` or `R` and is contravariant : we can assign a `T` to a `Either<T, null>`. It is convertible to `Variant<L, R>`.
* `Nullable<T>` which is `Either<T, null>`
* `Variant<A, B, ...Types>` which is `A`, `B` or any of the provided `Types`. It is convertible to a tree of `Either` (e.g. `Variant<A, B, C, D>` is also `Either<A, Either<B, Either<C, D>>>`).
* `Convertible<A, B>` which means that it is an `A` but can be converted to a `B` (not necessarily vice-versa)
* `T[]` means an array of `T`
* `Iterable<T>` is any iterable type whose values are of type `T`. It has a property `iterator` which is an `Iterator<T>`. It is a property as some iterable may only be traversed forward and once (e.g. seeded sequence)
* `Iterator<T>` is used to check validity and traverse its associated iterable
* `[A, B, C]` is a tuple whose first value is of type `A`, second value of type `B` and third value of type `C`. It is convertible to `Either<A, Either<B, C>>`.
* `Class<T>` a way to hold information of the type `T` so it can be used dynamically with `instanceof`. It also offers a method called `template <U> T cast(Convertible<U, T> value)` to cast to that type. `MyClass.cast` is also that method.

## Known implementations

* PHP: [voltra/lazy-collection](https://github.com/Voltra/lazy-collection)
