# Introduction #
You might come to the situation where you do not need all the entries of a csv file. This examples demonstrates how to apply a filter to the csv reader.

Consider you have the persons file from the previous examples and want to get all persons that are older than 18 years.

# The PersonEntryFilter #
The CSVEntryFilter interface is used to filter the entries. It shuold be self explaining.
```
public class AdultPersonFilter implements CSVEntryFilter<Person> {
  @Override
  public boolean match(Person p) {
    return p.getAge() >= 18;
  }
}
```

# Wire it all together #
When you build the CSVReader, you can provide the entry filter exactly as the entry parser. Once the reader is build, it will check each entry whether it maches the filter. If not, the next entry will be read immediately.
```
Reader csvFile = new InputStreamReader(Main.class.getResourceAsStream("/persons.csv"));

CSVReader<Person> personReader = new CSVReaderBuilder<Person>(csvFile).entryParser(new PersonEntryParser())
		.entryFilter(new AdultPersonFilter()).build();
List<Person> persons = personReader.readAll();
```