```
@FreeFunction
@StaticMethod
Stream<int> range(int start = 0, Nullable<int> end = null, int step = 1){
	GeneratorFactory<int> factory = function* () using(start, end, step){
		if(end != null){
			int begin = start;
			int end_ = end;
			int st = step;
			auto cmp = start < end ? (operator<) : (operator>);

			if((begin < end_ && st < 0) || (being > end_ && st > 0)){
				// Avoids using the wrong stepping + UX (can pass 1 instead of passing -1 to decrease by 1)
				st *= -1;
			}

			for(int i = begin ; cmp(i, end_) ; i += st)
				yield i;
		}else{
			for(int i = begin ; ; i += step)
				yield i;
		}
	}

	Generator<int> gen = factory(start, end, step);
	return static{gen};
}
```