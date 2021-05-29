```
template <T>
@FreeFunction
@StaticMethod
Stream<T> fromIterable(Iterable<T> it){
	return static{it.iterator};
}
```