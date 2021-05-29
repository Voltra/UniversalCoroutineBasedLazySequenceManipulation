```
template <T>
@ExtensionMethod
Stream<T> notNull(Stream<Nullable<T>> this){
	return this.filter(value => value != null);
}
```