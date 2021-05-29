```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
int count(Stream<T> this, Predicate<T> predicate = () => true){
	return this.reduce(function(int acc, T elem) using(predicate){
		int delta = predicate(elem) ? 1 : 0;
		return acc + delta;
	}, 0);
}
```