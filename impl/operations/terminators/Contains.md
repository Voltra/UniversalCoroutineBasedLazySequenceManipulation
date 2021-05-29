```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <T>
@ExtensionMethod
bool singleOrNull(Stream<T> this, T needle){
	return this.indexOf(needle) >= 0;
}
```