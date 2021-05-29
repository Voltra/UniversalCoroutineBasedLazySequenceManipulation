```
template <T, U>
class MapIterator<T, U> : Iterator<U>{
	constructor(Iterator<T> parent, Mapper<T, U> mapper){
		super();
		this.parent = parent;
		this.mapper = mapper;
	}

	isValid(){
		return parent.isValid();
	}

	current(){
		T elem = this.parent.current();
		return this.mapper(elem);
	}

	protected:
		Iterator<T> parent;
		Mapper<T, U> mapper;
}

template <T, U>
@ExtensionMethod
Stream<U> map(Stream<T> this, Mapper<T, U> mapper){
	return this.pipe(function(Iterator<T> parent) using(mapper){
		return MapIterator<T, U>{parent, mapper};
	});
}
```