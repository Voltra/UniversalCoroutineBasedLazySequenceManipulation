```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

template <A, B>
@ExtensionMethod
[A[], B[]] unzip(Stream<[A, B]> this){
	return this.reduce(function(auto acc, auto elem){
		auto [lhs, rhs] = elem;
		acc[0].push(lhs);
		acc[1].push(rhs);
		return acc;
	}, [[], []]);
}
```