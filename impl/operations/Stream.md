```
template <T>
class Stream<T> : Iterable<T> {
	type GeneratorType = Generator<T>;

	template <U>
	type GeneratorMapper<U> = (GeneratorType parent) => Generator<U>;

	constructor(GeneratorType gen){
		this.gen = gen;
	}

	protected:
		GeneratorType gen;

		template <U>
		Stream<U> pipe(GeneratorMapper<U> mapper){
			return ThisType{mapper(this.gen)};
		}
}
```