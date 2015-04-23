# Write a List of Es to a CSV file #
The CSVWriter uses a CSVEntryConverter to convert an object of type E to a String[.md](.md) array, that will be written to the csv file.
```
public interface CSVEntryConverter<E> {
	public String[] convertEntry(E e);
}
```

Once you have implemented this interface, you can build the CSVWriter instance. Like as in the previous example, you use the `CSVWriterBuilder` to configure your CSVWriter and provide the csv entry converter.
```
CSVWriter<...> csvWriter = new CSVWriterBuilder<...>(out).entryConverter(new YourEntryConverter()).build();
csvWriter.writeAll(listOfEs);
```

# Default implementation #
jCSV comes with a default implementation of the CSVEntryConverter, that processes String[.md](.md) arrays. This implementation may come in handy if you read the data using the default CSVReader.
```
public class DefaultCSVEntryConverter implements CSVEntryConverter<String[]> {
	@Override
	public String[] convertEntry(String[] data) {
		return data;
	}
}
```


# Incremental writing #
If you want to write a very large csv file and can not hold all your object instances in memory, you can write each entry separately using the `CSVWriter#write(E e)` method.
```
for (int i = 0; i < ONE_MILLION; i++) {
  Person p = newRandomPerson();
  csvWriter.write(person);
  csvWriter.flush();
}
```

# Examples #
  * [Write a csv file](CSVWriterExample.md)