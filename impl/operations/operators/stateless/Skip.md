```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
Stream<T> skipWhile(Stream<T> this, Predicate<T> predicate) {
	return this.pipe(function*(Generator<T> gen) using(predicate) {
		bool skip = true;

		foreach(value in gen){
			if(skip && predicate(value))
				continue;

			skip = false;
			yield value;
		}
	});
}

template <T>
@ExtensionMethod
Stream<T> skipUntil(Stream<T> this, Predicate<T> predicate) {
	return this.skipWhile(value => !predicate(value));
}

template <T>
@ExtensionMethod
Stream<T> skip(Stream<T> this, int maxAmount) {
	int count = 0;
	return this.skipWhile(() => count++ < maxAmount);
}
```