```
template <T, Callable>
@ExtensionMethod
Stream<T> uniqueBy(Stream<T> this, Callable idExtractor){
	Iterable<T> it = this.asIterable();

	GeneratorFactory<T> factory = function*() using(idExtractor, it){
		Iterable<T> unique = iterable__uniqueBy(it, idExtractor);
		yield* unique;
	};

	Generator<T> gen = factory();

	return static{gen};
}

template <T>
@ExtensionMethod
Stream<T> unique(Stream<T> this){
	return this.uniqueBy(value => value);
}
```