```
template <T, U>
@ExtensionMethod
Stream<[T, U]> zipWith(Stream<T> this, Iterable<U> it){
	return this.pipe(function*(Generator<T> parent) using(it){
		auto lhsIt = parent.iterator;
		auto rhsIt = it.iterator;

		while(lhsIt.isValid() && rhsIt.isValid()) {
			auto lhs = lhsIt.current();
			auto rhs = rhsIt.current();
			yield [lhs, rhs];
		}
	});
}
```