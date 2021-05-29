```
template <T>
class Stream<T> : Convertible<static, Iterable<T>> {
	type GeneratorType = Generator<T>;

	template <U>
	type GeneratorMapper<U> = (GeneratorType) => Generator<U>;

	constructor(GeneratorType gen){
		this.gen = gen;
	}

	Iterable<T> asIterable(){
		return this.gen;
	}

	operator Iterable<T>{
		return this.asIterable();
	}

	protected:
		GeneratorType gen;

		template <U>
		Stream<U> pipe(GeneratorMapper<U> mapper){
			return Stream<U>{mapper(this.gen)};
		}
}
```