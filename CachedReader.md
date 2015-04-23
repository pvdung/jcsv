# Cached reader #

You can extend an existing csv reader with a cache functionality. To create a cached csv reader, you simply call the appropriate factory method:
```
// your existing csv reader
CSVReader<Person> reader = ...;

// the cached csv reader
CachedCSVReader<Person> cachedReader = CSVReaderBuilder.cached(csvPersonReader);
```

The CachedCSVReader uses the specified CSVReader instance to read the records from the csv file. This means that an applied entry filter will affect the cached csv reader too.

The CachedCSVReader extends the ListIterator interface. You can move forward or backwards in your csv file. Once the csv reader reads and parses an entry, the entry will be cached by the CachedCSVReader instance. This means that all records are stored in memory. If you read a large csv file, you could get an OutOfMemoryException.

# Example #
  * [Use a cached reader](CachedReaderExample.md)