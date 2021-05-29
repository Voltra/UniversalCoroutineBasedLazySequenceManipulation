```
template <T, U = T>
@ExtensionMethod
Stream<T> instanceOf(Stream<T> this, Class<U> className){
	return this.filter(value => value instanceof className);
}

template <T, U = T>
@ExtensionMethod
Stream<U> asInstanceOf(Stream<T> this, Class<U> className){
	return this.instanceOf(className).map(value => className.cast(value));
}

template <T, U = T>
@ExtensionMethod
Stream<T> notInstanceOf(Stream<T> this, Class<U> className){
	return this.filterNot(value => value instanceof className);
}
```