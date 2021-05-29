```
template <T>
@ExtensionMethod
Stream<T> reverse(Stream<T> this){
	return this.pipe(function*(Generator<T> parent) using(callback){
		Iterable<T> reversed = iterable__reverse(parent);

		foreach(value in reversed)
			yield value;
	});
}
```