```
// Mapper is defined in /impl/operations/operators/stateless/Map.md

@ExtensionMethod
Number sum(Stream<Number> this, Number init = 0){
	return this.reduce((acc, elem) => acc + elem, 0);
}

@ExtensionMethod
Number sumBy(Stream<Number> this, Number init = 0){
	return this.reduce((acc, elem) => acc + elem, init);
}
```