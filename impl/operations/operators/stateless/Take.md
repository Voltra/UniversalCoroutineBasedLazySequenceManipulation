```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
Stream<T> takeWhile(Stream<T> this, Predicate<T> predicate) {
	return this.pipe(function*(Generator<T> gen) using(predicate) {
		foreach(value in gen){
			if(predicate(value))
				yield value;
			else
				break;
		}
	});
}

template <T>
@ExtensionMethod
Stream<T> takeUntil(Stream<T> this, Predicate<T> predicate) {
	return this.takeWhile(value => !predicate(value));
}

template <T>
@ExtensionMethod
Stream<T> take(Stream<T> this, int maxAmount) {
	int count = 0;
	return this.takeWhile(() => count++ < maxAmount);
}
```