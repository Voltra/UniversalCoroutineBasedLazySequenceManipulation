```
@FreeFunction
@StaticMethod
Stream<String> splitBy(String str, String sep = "", bool removeEmptyStrings = true){
	Iterable<String> splat = string__splitBy(str, sep);
	auto predicate = removeEmptyStrings ? str => str != "" : () => true;

	return static::fromIterable(splat).filter(predicate);
}
```