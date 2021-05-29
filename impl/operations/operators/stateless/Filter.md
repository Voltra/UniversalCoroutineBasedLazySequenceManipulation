```
// Mapper is defined in /impl/operations/operators/stateless/Map.md

template <T>
type Predicate<T> = Mapper<T, bool>;

template <T>
@ExtensionMethod
Stream<T> filter(Stream<T> this, Predicate<T> predicate){
	return this.pipe(function*(Generator<T> parent) using(predicate){
		foreach(value in parent){
			auto shouldYield = predicate(value);

			if(shouldYield)
				yield value;
		}
	});
}

template <T>
@ExtensionMethod
Stream<T> filterNot(Stream<T> this, Predicate<T> predicate){
	return this.filter(value => !predicate(value));
}

template <T>
@ExtensionMethod
Stream<T> filterOut(Stream<T> this, Predicate<T> predicate){
	return this.filterNot(predicate);
}
```