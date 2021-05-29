```
template <T>
class JoinOptions<T>{
	String prefix = "[";
	String separator = ",";
	String suffix = "[";
	Mapper<T, String> stringifier;

	constructor(Mapper<T, String> stringifier){
		this.stringifier = stringifier;
	}
}

template <T>
class JoinOptions<Convertible<T, String>> : JoinOptions<T>{
	constructor(){
		super(String.cast);
	}
}


template <T>
@ExtensionMethod
String join(Stream<T> this, JoinOptions<T> joinOptions){
	String ret = joinOptions.prefix;
	bool hasElems = false;

	foreach(value in this){
		String tmp = joinOptions.stringifier(value);
		ret += tmp + joinOptions.separator;
		hasElems = true;
	}

	int sepLength = joinOptions.separator.length;
	if(hasElems && sepLength > 0)
		ret = ret.substr(0, -sepLength);

	ret += joinOptions.suffix;
	return ret;
}
```