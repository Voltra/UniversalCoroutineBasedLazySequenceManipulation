```
// Predicate is defined in /impl/operations/operators/stateless/Filter.md

@ExtensionMethod
Number average(Stream<Number> this){
	int i = 0;

	return this.reduce(function(Number acc, Number elem) using(i){
		//cf. https://stackoverflow.com/a/5984099/7316365

		int key = i++;
		return ((acc * key) + elem) / i;
	}, 0);
}
```