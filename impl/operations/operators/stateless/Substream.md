```
template <T>
@ExtensionMethod
Stream<T> substream(Stream<T> this, int startIndex, int maxEndIndex) {
	int take = maxEndIndex - startIndex;
	return this.skip(startIndex).take(take);
}
```