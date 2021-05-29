```
template <T, Callable>
@ExtensionMethod
Stream<T[]> chunks(Stream<T> this, int maxSize){
	T[] bucket = [];

	return this.pipe(function*(Generator<T> parent) using(bucket, size){
		foreach(value in parent){
			if(bucket.length < size){
				bucket.push(value);
			}else{
				yield bucket;
				bucket = [value];
			}
		}

		if(bucket.length > 0) // Do not forget cleanup for non-evened streams
			yield bucket;
	});
}
```