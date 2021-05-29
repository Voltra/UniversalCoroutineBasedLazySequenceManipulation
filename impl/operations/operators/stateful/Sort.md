```
template <T, U = T>
type Comparator<T, U> = (T, U) => int;

template <T, Callable>
@ExtensionMethod
Stream<T> sortBy(Stream<T> this, Callable idExtractor){
	Iterable<T> it = this.asIterable();

	GeneratorFactory<T> factory = function*() using(idExtractor, it){
		Iterable<T> sorted = iterable__sortAscBy(it, idExtractor);
		yield* sorted;
	};

	Generator<T> gen = factory();

	return static{gen};
}

template <T>
@ExtensionMethod
Stream<T> sort(Stream<T> this){
	return this.sortBy(value => value);
}

template <T, Callable>
@ExtensionMethod
Stream<T> sortByDescending(Stream<T> this, Callable idExtractor){
	Iterable<T> it = this.asIterable();

	GeneratorFactory<T> factory = function*() using(idExtractor, it){
		Iterable<T> sorted = iterable__sortDescBy(it, idExtractor);
		yield* sorted;
	};

	Generator<T> gen = factory();

	return static{gen};
}

template <T>
@ExtensionMethod
Stream<T> sortDescending(Stream<T> this){
	return this.sortByDescending(value => value);
}

template <T>
@ExtensionMethod
Stream<T> sortWith(Stream<T> this, Comparator<T> comparator){
	Iterable<T> it = this.asIterable();

	GeneratorFactory<T> factory = function*() using(comparator, it){
		Iterable<T> sorted = iterable__sortWith(it, comparator);
		yield* sorted;
	};

	Generator<T> gen = factory();

	return static{gen};
}
```