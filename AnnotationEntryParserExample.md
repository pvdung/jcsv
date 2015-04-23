# Introduction #

Consider you have the same csv file as stated in the first example.
```
Holger;Schmidt;35
Max;Mustermann;17
Lisa;Stein;19
```
If you want to parse such a file with an annotated class, you have only have to provide a class that maps it's field to the columns of the file.

# The person class #
The person class in the first example was quite simple. You now have to add some additional information to it. The MapToColumn annotation does this job.
```
public class AnnotatedPerson {

  @MapToColumn(column=0)
  private String firstname;
  @MapToColumn(column=1)
  private String lastname;
  @MapToColumn(column=2)
  private int age;

  @Override
  public String toString() {
    return String.format("%s %s %s years", firstname, lastname, age);
  }
}
```

# Wire it all together #
Now you can build a CSVReader that uses your annotated class.
```
Reader csvFile = new InputStreamReader(Main.class.getResourceAsStream("/persons.csv"));

ValueProcessorProvider vpp = new ValueProcessorProvider();
CSVReader<Person> personReader = new CSVReaderBuilder<Person>(csvFile).entryParser(
		new AnnotationEntryParser<Person>(Person.class, vpp)).build();
List<Person> persons = personReader.readAll();
```
That's it. :)