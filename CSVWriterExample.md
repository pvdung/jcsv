# Introduction #
Consider we would like to create a `persons.csv` file like the one from the reader examples.

In our application, we have a list of persons:
```
List<Person> persons = new ArrayList<Person>();
persons.add(new Person("Holger", "Schmidt", 35));
persons.add(new Person("Max", "Mustermann", 17));
persons.add(new Person("Lisa", "Stein", 19));
```

You can write this data to a csv file using a CSVWriter.

# The PersonEntryConverter #
The CSVEntryConverter for our Person class could look like this:
```
public class PersonEntryConverter implements CSVEntryConverter<Person> {
  @Override
  public String[] convertEntry(Person p) {
    String[] columns = new String[3];

    columns[0] = p.getFirstName();
    columns[1] = p.getLastName();
    columns[2] = String.valueOf(p.getAge());

    return columns;
  }
}
```

# Wire it all together #
We can now build the CSVWriter instance and provide our entry converter implementation.
```
Writer out = new FileWriter("persons.csv");

CSVWriter<Person> csvWriter = new CSVWriterBuilder<Person>(out).entryConverter(new PersonEntryConverter()).build();
csvWriter.writeAll(persons);
```