```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
Nullable<T> lastOrNull(Stream<T> this, Predicate<T> predicate = () => true){
	Nullable<T> last = null;

	foreach(value in this){
		if(predicate(value))
			last = value;
	}

	return last;
}

template <T, U = T>
@ExtensionMethod
Either<T, U> lastOr(Stream<T> this, U defaultValue, Predicate<T> predicate = () => true){
	Nullable<T> value = this.lastOrNull(predicate);
	return value == null ? defaultValue : value;
}

template <T>
@ExtensionMethod
T first(Stream<T> this, Predicate<T> predicate = () => true){
	Nullable<T> value = this.lastOrNull(predicate);

	if(value == null)
		throw NotFound{};

	return value;
}
```