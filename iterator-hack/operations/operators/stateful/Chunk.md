```
template <T>
class ChunksIterator<T> : Iterator<T[]> {
	constructor(Iterator<T> parent, int maxBucketSize){
		super();
		this.parent = parent;
		this.bucket = [];
		this.maxBucketSize = maxBucketSize;
	}

	isValid(){
		return this.bucket.length != 0 || parent.isValid();
	}

	current(){
		while(this.isValid()){
			if(this.bucket.length < this.maxBucketSize){
				bucket.push(value);
			}else{
				auto ret = [...bucket];
				bucket = [value];
				return bucket;
			}
		}

		return this.bucket;
	}

	protected:
		Iterator<T> parent;
		T[] bucket;
		int maxBucketSize;
}

template <T>
@ExtensionMethod
Stream<T[]> chunks(Stream<T> this, int maxSize){
	return this.pipe(function(Iterator<T> parent) using(bucket, maxSize){
		return ChunksIterator<T>{parent, maxSize};
	});
}
```