```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
[T[], T[]] partition(Stream<T> this, Predicate<T> predicate){
	return this.reduce(function(auto acc, T elem){
		int index = predicate(elem) ? 0 : 1;
		acc[index].push(elem);
		return acc;
	}, [[], []]);
}
```