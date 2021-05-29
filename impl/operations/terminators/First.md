```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
Nullable<T> firstOrNull(Stream<T> this, Predicate<T> predicate = () => true){
	foreach(value in this){
		if(predicate(value))
			return value;
	}

	return null;
}

template <T, U = T>
@ExtensionMethod
Either<T, U> firstOr(Stream<T> this, U defaultValue, Predicate<T> predicate = () => true){
	Nullable<T> value = this.firstOrNull(predicate);
	return value == null ? defaultValue : value;
}

template <T>
@ExtensionMethod
T first(Stream<T> this, Predicate<T> predicate = () => true){
	Nullable<T> value = this.firstOrNull(predicate);

	if(value == null)
		throw NotFound{};

	return value;
}
```