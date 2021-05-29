```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
Nullable<T> atIndexOrNull(Stream<T> this, int index){
	int cursor = 0;
	return this.firstOrNull(() => cursor++ == index);
}

template <T, U = T>
@ExtensionMethod
Either<T, U> atIndexOr(Stream<T> this, int index, U defaultValue){
	Nullable<T> value = this.atIndexOrNull(index);
	return value == null ? defaultValue : value;
}

template <T>
@ExtensionMethod
T atIndex(Stream<T> this, int index){
	Nullable<T> value = this.atIndexOrNull(index);

	if(value == null)
		throw NotFound{};

	return value;
}
```