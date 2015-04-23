# Introduction #

We extend the csv file from the previous examples with a fourth column that states the gender of the person:
```
Holger;Schmidt;35;male
Max;Mustermann;17;male
Lisa;Stein;19;female
```

The modified person class looks like this:
```
public class AnnotatedPerson {
  @MapToColumn(column=0)
  private String firstname;
  @MapToColumn(column=1)
  private String lastname;
  @MapToColumn(column=2)
  private int age;
  @MapToColumn(column=3)
  private Gender gender;
	
  @Override
  public String toString() {
    return String.format("%s %s %s years %s", firstname, lastname, age, gender);
  }
}
```

# The GenderValueProcessor #
jCSV has no information how to process the fourth column of the enum type Gender.
You can provide this information by adding a ValueProcessor for the type Gender.
```
public class GenderValueProcessor implements ValueProcessor<Gender> {
  @Override
  public Gender processValue(String value) {
    if (value.equalsIgnoreCase("male")) {
      return Gender.MALE;
    } else if (value.equalsIgnoreCase("female")) {
      return Gender.FEMALE;
    }
		
    throw new IllegalArgumentException(value + " is not a valid gender");
  }
}
```

# Wire it all together #
You have to register the GenderValueProcessor in the ValueProcessorProvider and then create the CSVReader.
```
ValueProcessorProvider vpp = new ValueProcessorProvider();
vpp.registerValueProcessor(Gender.class, new GenderValueProcessor());

Reader csvFile = new InputStreamReader(Main.class.getResourceAsStream("/persons.csv"));

CSVReader<Person> csvReader = new CSVReaderBuilder<Person>(csvFile).entryParser(
		new AnnotationEntryParser<Person>(Person.class, vpp)).build();

List<Person> persons = csvReader.readAll();
```
In the most cases, the default value processors should fit your needs. jCSV comes with value processors for the primitives (and its wrapper types), the String class and the Date class. The DateValueProcessor by default uses the SimpleDateFormat for the system locale to parse the strings.