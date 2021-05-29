```
template <T>
@ExtensionMethod
Stream<T> truthy(Stream<Convertible<T, bool>> this){
	return this.filter(value => !!value);
}

template <T>
@ExtensionMethod
Stream<T> falsy(Stream<Convertible<T, bool>> this){
	return this.filter(value => !value);
}
```