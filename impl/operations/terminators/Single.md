```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
Nullable<T> singleOrNull(Stream<T> this, Predicate<T> predicate = () => true){
	T[] arr = this.filter(predicate).toArray();

	return arr.length == 1 ? arr[0] : null;
}

template <T, U = T>
@ExtensionMethod
Either<T, U> singleOr(Stream<T> this, U defaultValue, Predicate<T> predicate = () => true){
	Nullable<T> value = this.singleOrNull(predicate);
	return value == null ? defaultValue : value;
}

template <T>
@ExtensionMethod
T single(Stream<T> this, Predicate<T> predicate = () => true){
	Nullable<T> value = this.singleOrNull(predicate);

	if(value == null)
		throw NotFound{};

	return value;
}
```