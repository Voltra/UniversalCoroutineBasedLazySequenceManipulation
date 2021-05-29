```
template <T>
@ExtensionMethod
Stream<T> flatten(Stream<Either<T, Iterable<T>>> this){
	return this.pipe(function*(Generator<T> parent) using(callback){
		foreach(value in parent){
			if(iterable__isIterable(value))
				yield* value;
			else
				yield value;
		}
	});
}
```