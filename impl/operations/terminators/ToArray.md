```
template <T>
@ExtensionMethod
T[] count(Stream<T> this){
	return this.reduce(function(T[] acc, T elem){
		acc.push(elem);
		return acc;
	}, []);
}
```