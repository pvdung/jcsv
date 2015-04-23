# Table of contents #


# Introduction #

Consider you have the following CSV file:
```
Holger;Schmidt;35
Max;Mustermann;17
Lisa;Stein;19
```
Each line represents a person. Each person has a first name, a last name and an age. Nothing exciting this far...

It is very easy to parse this csv file using jCSV. It comes with a fully configured CSVReader that does (mostly) all of the work.

# Parse the data #
The first thing to do, is to read the csv file and translate it into java objects, so that the data can ba used in your program.

There are currently three ways to do this:
  * parse the data as a String[.md](.md) array
  * create Java objects using the CSVEntryParser
  * create Java objects using an annotated class
Each of these approaches is useful for specific situation,

## Parse the data as a String[.md](.md) array ##
A straightforward approach would be to read the file line by line. Each line would be split at ';', so we would receive an String[.md](.md) array containing the three data parts.

The default CSVParser configuration follows this approach, you can obtain this parser using the static factory method `CSVReaderBuilder.newDefaultReader(Reader reader)`.

An example code might look like this:
```
Reader reader = new FileReader("persons.csv");

CSVReader<String[]> csvPersonReader = CSVReaderBuilder.newDefaultReader(reader);
List<String[]> persons = csvPersonReader.readAll();
```
You see, it's quite easy to read a CSV file and use the data in your program.

## Create Java objects using the CSVEntryParser ##

But it would be a lot easier to use a List of Person objects in your program. jCSV can achieve this for you, it can convert each row into a java object.

All we have to do is to tell jCSV how to convert a row from the csv file into a Person object. For this purpose exists an interface called CSVEntryParser:
```
public interface CSVEntryParser<E> {
	public E parseEntry(String... data);
}
```

The String[.md](.md) array represents the columns of the csv file. The method `parseEntry` converts this String[.md](.md) array to an object of the class E, E will be Person in our example. So all you have to do, is to provide an appropiate CSVEntryParser, a straightforward implementation might look like this:
```
public class PersonEntryParser implements CSVEntryParser<Person> {
	public Person parseEntry(String... data) {
		String firstname = data[0];
		String lastname = data[1];
		int age = Integer.parseInt(data[2]);

		return new Person(firstname, lastname, age);
	}
}
```

Once you have written this entry parser, you can use the CSVReaderBuilder to provide your CSVEntryParser implementation:
```
Reader reader = new FileReader("persons.csv");

CSVReader<Person> csvPersonReader = new CSVReaderBuilder<Person>(reader).entryParser(new PersonEntryParser()).build();
List<Person> persons = csvPersonReader.readAll();
```

That's it, you have completely parsed a csv file in just 4 lines!

## Create Java objects using annotations ##
Instead of implementing an entry parser, you can instruct jCSV with an annotated class, that specifies how to build the Person objects.
All we have to do is to map the columns from the csv file to the class's properties. This mapping is done via annotations in the class that we would like to build. The annotation we use for this purpose is `MapToColumn`, it comes with two parameter:
  * column (required), the column in the csv file
  * type (optional), the type of the data
The column is an integer, where the type is a class. If you do not specify the type of the property, jCSV will determine the type from the type of the field that holds the annotation.

The annotated Person class may look like this:
```
public class Person {
	@MapToColumn(column=0)
	private String firstName;

	@MapToColumn(column=1)
	private String lastName;

	@MapToColumn(column=2)
	private int age;

	// getter, equals, toString, ...
}
```
This code should be self explaining if you look at the csv file above. You now have to provide this information to jCSV via the AnnotationEntryParser:
```
Reader reader = new FileReader("persons.csv");

ValueProcessorProvider provider = new ValueProcessorProvider();
CSVEntryParser<Person> entryParser = new AnnotationEntryParser<Person>(Person.class, provider);
CSVReader<Person> csvPersonReader = new CSVReaderBuilder<Person>(reader).entryParser(entryParser).build();

List<Person> persons = csvPersonReader.readAll();
```

Thats all. There is no need to implement an entry parser anymore.

jCSV uses reflection to set the values, so there are a few drawbacks if you choose to parse your csv using annotations:
  * The properties have to be non-final
  * There has to be an accessible default constructor

### Value processors ###
jCSV uses value processors to convert the String data to the needed type. As you can see in the example above, the third column is mapped to an int property. jCSV now uses the IntegerProcessor to convert the String into an Integer. There are several default value processors, such as primitives (and its wrapper types), a StringProcessor and a DateProcessor that uses the default SimpleDateFormat.

If these value processors are not enough, you can simply add your own value processors. Consider you have a column that holds an address and should be converted to an Address object. The value processor for this type might look look this:
```
public class AddressProcessor implements ValueProcessor<Address> {
	@Override
	public Address processColumn(String value) {
		Address address = toAddress(value);
		return address;
	}
}
```
To publish this value processor, simply call:
```
ValueProcessorProvider vpp = new ValueProcessorProvider();
vpp.registerValueProcessor(Address.class, new AddressProcessor());
```

However, in the most cases, you do not have to provide your own ValueProcessor. The default implementations of the primitives, it's wrapper classes, String and Date should be enough.

# Read the entries #
After configuring the CSVReader, you can access the data. As you may have seen above, one apprach is to read the whole data into a List. This can be done, calling the readAll() method.
```
List<Person> persons = csvReader.readAll();
```

If you have a large csv file, you possibly can not read the whole csv file into your memory. Therefore, there is the possbility to incrementaly read the records of the file.

The CSVReader interfaces extends the Iterable interface, so you can simply iterate over the records using an Iterator:
```
Iterator<Person> it = csvPersonReader.iterator();
while (it.hasNext()) {
	Person p = it.next();
	// ...
}
```

Another option is to use the for each loop:
```
for (Person p : csvPersonReader) {
	/// ...
}
```

# Examples #
  * [Read a csv file (with a CSVEntryParser)](CSVEntryParserExample.md)
  * [Read a csv file (with annotations)](AnnotationEntryParserExample.md)