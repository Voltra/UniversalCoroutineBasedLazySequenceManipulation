```
// Callback is defined in /impl/operations/operators/stateless/Peek.md

template <T>
@ExtensionMethod
void forEach(Stream<T> this, Callback<T> callback){
	foreach(value in this){
		callback(value);
	}
}
```