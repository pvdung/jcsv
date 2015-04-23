# Build the cached reader #
You can create a cached reader by calling the static factory method provided by the _CSVReaderBuilder_
```
CachedCSVReader<Person> cachedReader = CSVReaderBuilder.cached(personReader);
```

The cached reader now holds all read entries in memory. That allows very fast access to your data. It will be useful when you have to read your data multiple times.