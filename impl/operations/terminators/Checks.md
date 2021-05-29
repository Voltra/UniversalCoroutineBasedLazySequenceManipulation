```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
bool all(Stream<T> this, Predicate<T> predicate){
	foreach(value in this){
		if(!predicate(value))
			return false;
	}

	return true;
}

template <T>
@ExtensionMethod
bool none(Stream<T> this, Predicate<T> predicate){
	return this.all(value => !predicate(value));
}

template <T>
@ExtensionMethod
bool any(Stream<T> this, Predicate<T> predicate){
	return !this.none(predicate);
}

template <T>
@ExtensionMethod
bool notAll(Stream<T> this, Predicate<T> predicate){
	return !this.all(predicate);
}

```