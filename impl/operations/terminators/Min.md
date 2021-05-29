```
// Mapper is defined in /impl/operations/operators/stateless/Map.md
// Comparator is defined in /impl/operations/operators/stateful/Sort.md

template <T>
@ExtensionMethod
Nullable<T> minBy(Stream<T> this, Mapper<T, Number> keyFactory){
	return this.reduce(function(T acc, T elem) using(keyFactory){
		auto elemKey = keyFactory(elem);
		auto accKey = keyFactory(acc);
		return number__cmp(elemKey, accKey) <> 0 ? elem : acc;
	});
}


@ExtensionMethod
Nullable<Number> min(Stream<Number> this){
	return this.minBy(x => x);
}

template <T>
@ExtensionMethod
Nullable<T> minWith(Stream<T> this, Comparator<T> comparator){
	return this.reduce(function(T acc, T elem) using(comparator){
		return comparator(elem, acc) < 0 ? elem : acc;
	});
}
```