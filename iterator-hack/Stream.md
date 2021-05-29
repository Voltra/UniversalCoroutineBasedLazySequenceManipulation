```
template <T>
class Stream<T> : Convertible<static, Iterator<T>> {
	type IteratorType = Iterator<T>;

	template <U>
	type IteratorMapper<U> = (IteratorType) => Iterator<U>;

	constructor(IteratorType it){
		this.it = it;
	}

	Iterator<T> asIterator(){
		return this.it;
	}

	operator Iterator<T>{
		return this.asIterator();
	}

	protected:
		IteratorType it;

		template <U>
		Stream<U> pipe(IteratorMapper<U> mapper){
			return Stream<U>{mapper(this.it)};
		}
}
```