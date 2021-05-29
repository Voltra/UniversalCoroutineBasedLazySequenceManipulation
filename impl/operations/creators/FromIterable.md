```
template <T>
@FreeFunction
@StaticMethod
Stream<T> fromIterable(Iterable<T> it){
	GeneratorFactory<T> factory = function*() using(it){
		yield* it;
	};

	Generator<T> gen = factory();

	return static{gen};
}
```