```
@FreeFunction
@StaticMethod
Stream<String> splitByRegex(String str, Regex re, bool removeEmptyStrings = true){
	Iterable<String> splat = regex__split(re, str);
	auto predicate = removeEmptyStrings ? str => str != "" : () => true;

	return static::fromIterable(splat).filter(predicate);
}
```