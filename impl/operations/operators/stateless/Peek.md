```
// Mapper is defined in /impl/operations/operators/stateless/Map.md

template <T>
type Callback<T> = Mapper<T, void>;

template <T>
@ExtensionMethod
Stream<T> peek(Stream<T> this, Callback<T> callback){
	return this.pipe(function*(Generator<T> parent) using(callback){
		foreach(value in parent){
			callback(value);
			yield value;
		}
	});
}
```