```
template <T>
@ExtensionMethod
Stream<T> then(Stream<T> this, Iterable<T> it){
	return this.pipe(function*(Generator<T> parent) using(it){
		yield* parent;
		yield* it;
	});
}
```