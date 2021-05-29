```
// Mapper is defined in /impl/operations/operators/stateless/Map.md

template <T, U>
type FlatMapper<T, U> = Mapper<
	T,
	Either<U, Iterable<U>>
>;

template <T, U>
@ExtensionMethod
Stream<U> map(Stream<T> this, FlatMapper<T, U> mapper){
	return this.map(mapper).flatten();
}

template <T, U>
@ExtensionMethod
Stream<U> flatten(Stream<Either<T, Iterable<T>>> this, Mapper<T, U> mapper){
	return this.flatten().map(mapper);
}
```