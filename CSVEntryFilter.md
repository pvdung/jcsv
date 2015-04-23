# Introduction #

jCSV allows you to apply a filter on the CSVReader. The CSVReader then only reads entries that matches your filter.
This may come in handy, if you have a large csv file and you only want to look on a small portion of the data.

jCSV provides an interface for this purpose:
```
public interface CSVEntryFilter<E> {
    public boolean match(E e);
}
```

# How to create an entry filter #
Image you have a persons.csv file that holds the data of a large amount of persons. Each person has a name and an age.
You are only interested in persons that are younger than 18 years. When you build the CSVReader, you can simply apply a filter that fits this purpose. The filter mights look like this:
```
public class ChildrenFilter implements CSVEntryFilter<Person> {
    @Override
    public boolean match(Person person) {
        return person.getAge() < 18;
    }
}
```
This filter can be applied like this:
```
CSVReader<Person> csvPersonReader = new CSVReaderBuilder<Person>(fileReader).entryFilter(new ChildrenFilter())...
```
If you call the readNext() method, the CSVReader will read entries as long as an entry has been found that matches the applied filter.
The readAll() Method also uses the applied filter, you will receive a list of all records that matches the filter.

# Examples #
  * [Filter some entries](CSVEntryFilterExample.md)