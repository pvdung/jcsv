# The CSVStrategy #
You can specify your own csv format by passing a CSVStrategy object to the CSVReader:
```
new CSVReaderBuilder<MyClass>.(csvFileReader).strategy(myStrategy)...;
```

The CSVStrategy specifies the format of the csv file and the behaviour of the CSVReader. There are various parameters that can be configured:
  * `delimiter` _char_: the seperator of the tokens
  * `quoteCharacter` _char_: the quote character
  * `commentIndicator` _char_: the comment indicator char
  * `skipHeader` _boolean_: if true, skips the first line
  * `ignoreEmptyLines` _boolean_: if true, ignores empty lines

### Defaults ###

jCSV comes with some default implementations for the CSVStrategy:
| name | delimiter | quote character | comment indicator | skip headers? | ignore empty lines |
|:-----|:----------|:----------------|:------------------|:--------------|:-------------------|
| `CSVStrategy.DEFAULT` | ; | " | # | no | yes |
| `CSVStrategy_UK_DEFAULT` | , | " | # | no | yes |

If you don't specifiy a CSVStrategy, `CSVStrategy.DEFAULT` will be used as default.