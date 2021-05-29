```
// Mapper is defined in /impl/operations/operators/stateless/Map.md

template <T, Key = T>
@ExtensionMethod
Map<Key, T[]> groupBy(Stream<T> this, Mapper<T, Key> keyFactory){
	return this.reduce(function(Map<Key, T[]> acc, T elem) using(keyFactory){
		Key key = keyFactory(elem);
		T[] values = acc.getOrDefault(key, []);
		values.push(elem);
		acc.set(key, values);
		return acc;
	}, {});
}
```