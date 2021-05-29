```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
Nullable<int> indexOfFirst(Stream<T> this, Predicate<T> predicate){
	int cursor = 0;

	foreach(value in this){
		if(predicate(value))
			return cursor;

		cursor++;
	}

	return null;
}

template <T>
@ExtensionMethod
Nullable<int> indexOf(Stream<T> this, T needle){
	return this.indexOfFirst(value => value == needle);
}

template <T>
@ExtensionMethod
Nullable<int> indexOfLast(Stream<T> this, Predicate<T> predicate){
	int cursor = 0;
	Nullable<int> ret = null;

	foreach(value in this){
		if(predicate(value))
			ret = cursor;

		cursor++;
	}

	return ret;
}

template <T>
@ExtensionMethod
Nullable<int> lastIndexOf(Stream<T> this, T needle){
	return this.indexOfLast(value => value == needle);
}
```