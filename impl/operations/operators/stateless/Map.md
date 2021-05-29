```
template <T, U>
type Mapper<T, U> = (T) => U;

template <T, U>
@ExtensionMethod
Stream<U> map(Stream<T> this, Mapper<T, U> mapper){
	return this.pipe(function*(Generator<T> parent) using(mapper){
		foreach(value in parent)
			yield mapper(value);
	});
}
```