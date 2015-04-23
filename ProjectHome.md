jCSV is a very easy to use csv library for Java. With just a few lines of Code you can parse any csv file.
```
Reader reader = new FileReader("data.csv");

CSVReader<String[]> csvParser = CSVReaderBuilder.newDefaultReader(reader);
List<String[]> data = csvParser.readAll();
```

jCSV also provides mechanisms to bind the csv file directly to java objects.
```
Reader reader = new FileReader("persons.csv");

CSVReader<Person> csvPersonReader = ...;

// read all entries at once
List<Person> persons = csvPersonReader.readAll();

// read each entry individually
Iterator<Person> it = csvPersonReader.iterator();
while (it.hasNext()) {
  Person p = it.next();
  // ...
}
```
See the wiki for a working example.

jCSV fully supports the csv standard that is described in:
[Comma-separated values - Wikipedia](http://en.wikipedia.org/wiki/Comma-separated_values)

More features:
  * filter entries, see [CSVEntryFilter - Wiki](http://code.google.com/p/jcsv/wiki/CSVEntryFilter)
  * fully configurable format, see [CSVStrategy - Wiki](http://code.google.com/p/jcsv/wiki/CSVStrategy)
  * parse a csv file via Annotations, see [CSVReader - Wiki](CSVReader#Create_Java_objects_using_annotations.md)

### Maven ###
If you have a Maven project, you can add a dependency on jCSV to your project.
```
<dependency>
	<groupId>com.googlecode.jcsv</groupId>
	<artifactId>jcsv</artifactId>
	<version>1.4.0</version>
</dependency>
```