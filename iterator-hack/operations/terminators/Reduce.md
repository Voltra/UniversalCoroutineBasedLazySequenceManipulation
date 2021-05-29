```
template <Acc, T>
type Reducer<Acc, T> = (Acc, T) => Acc;

template <T, Acc = T>
@ExtensionMethod
Nullable<Acc> reduce(Stream<T> this, Reducer<Acc, T> reducer, Nullable<Acc> init = null){
	Nullable<Acc> acc = init;
	bool firstDone = acc != null;

	while(this.it.isValid()){
		T value = this.it.current();
		if(!firstDone){
			acc = value;
			firstDone = true;
		}else
			acc = reducer(acc, value);
	}

	return acc;
}
```