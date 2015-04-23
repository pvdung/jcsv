# Introduction #

Consider you have the following csv file:
```
Holger;Schmidt;35
Max;Mustermann;17
Lisa;Stein;19
```
Each line represents a person. Each person has a first name, a last name and an age.

You now have to create a class that represents a person and an entry parser that converts a row of the csv file to a person object.

# The person class #
Let's start with the person class:
```
public class Person {
  private final String firstname;
  private final String lastname;
  private final int age;
	
    public Person(String firstname, String lastname, int age) {
    this.firstname = firstname;
    this.lastname = lastname;
    this.age = age;
  }
	
  // getter, equals, hashcode
	
  @Override
  public String toString() {
    return String.format("%s %s %s years", firstname, lastname, age);
  }
}
```

# The PersonEntryParser #
You have to implement the generic CSVEntryParser interface that says how to convert the data.
The PersonEntryParser for our example may look like this:
```
public class PersonEntryParser implements CSVEntryParser<Person> {
  @Override
  public Person parseEntry(String... data) {
    if (data.length != 3) {
      throw new IllegalArgumentException("data is not a valid person record");
    }
		
    String firstname = data[0];
    String lastname = data[1];
    int age = Integer.parseInt(data[2]);
		
    return new Person(firstname, lastname, age);
  }
}
```
The entry parser will be called for each row of the csv file. The String array contains the data of that row.

# Wire it all together #
Now that we have prepared all the necessary stuff, we can build a csv reader and read the data.
```
Reader csvFile = new InputStreamReader(Main.class.getResourceAsStream("/persons.csv"));

CSVReader<Person> personReader = new CSVReaderBuilder<Person>(csvFile).entryParser(new PersonEntryParser()).build();
List<Person> persons = personReader.readAll();
```

That's it. Now the persons list contains the three persons from our csv file above :)

# Use the default CSVEntryParser #
Although it is recommended, you do not have to specify your own entry parser. jCSV comes with a default CSVEntryParser that parses the data as a String[.md](.md) array.
```
Reader csvFile = new InputStreamReader(Main.class.getResourceAsStream("/persons.csv"));

CSVReader<String[]> personReader = CSVReaderBuilder.newDefaultReader(csvFile);
List<String[]> persons = personReader.readAll();
```