```
// Mapper is defined in /impl/operations/operators/stateless/Map.md

template <T, Key = T, Value = T>
@ExtensionMethod
Map<Key, Value> associateBy(Stream<T> this, Mapper<T, Key> keyFactory, Mapper<T, Value> valueFactory){
	return this.reduce(function(Map<Key, Value> acc, T elem) using(keyFactory, valueFactory){
		acc.set(keyFactory(elem), valueFactory(elem));
		return acc;
	}, {});
}

template <T, Key = T, Value = T>
@ExtensionMethod
Map<Key, Value> associate(Stream<T> this, Mapper<T, [Key, Value]> entryFactory){
	return this.reduce(function(Map<Key, Value> acc, T elem) using(entryFactory){
		auto [key, value] = entryFactory(elem);
		acc.set(key, value);
		return acc;
	}, {})
}

template <A, B>
@ExtensionMethod
Map<A, B> associateUnzip(
	Stream<[A, B]> this,
	Mapper<[A, B], A> keyFactory = arr => arr[0],
	Mapper<[A, B], B> valueFactory = arr => arr[1]
){
	return this.associateBy(keyFactory, valueFactory);
}
```