## Processing Log for Hourly Rainfall Data


### Overview

While working with ARIMA models, my model was being trained but it was not able to do any forecasting. I received the following error:

```
 ValueWarning: A date index has been provided, but it has no associated frequency information and so will be ignored when e.g. forecasting.
  ' ignored when e.g. forecasting.', ValueWarning)
```

Digging around Stack Overflow and other sources led me to determine that my data was missing a frequency. This was not obvious at first since I had a plethora of data recorded for roughly 10 years. But as it happens, ARIMA needs to have all values of a given period represented, even if they have no observations. My data was only being recorded on days there was rain, specifically the hours rain was occurring. This lead to a fair amount of gaps in the data.

### Solution

The solution was to create an uninterrupted series of dates from 2004-01-01 through 2014-01-01. Days that were absent from the original dataset were backfilled and given a corresponding value of zero. Days with a zero value will not corrupt or skew the data since by virtue of those dates not being recorded, had zero levels of rainfall to observe.
As for days where hourly rainfall was observed, I truncated the timestamp expression so that the date only reflected the calendar day. Additionally, I concatenated all obersvations for a given day expressing the sum of the rain observed on the day rather than including hourly values. Aside from allowing for a frequency to be applied this had the added benefit of shrinking my dataset and making it slightly more manageable.


### Methodology

* Read the original dataset in as a csv, while parsing dates 
* Write the dataframe out to a new file 
	* Reason for this that the original dataset would not allow the date notation to be coerced in Excel
* Create a pivot table to sum the HPCP values for the days on which rain was observed
* Save the output to a new Excel workbook 
* To backfill the missing dates, create essentially a vector of all possible dates in the time range in a separate sheet in the same workbook
* Run a matching function to print whether or not the date is missing in the data on the main sheet
* Filter by all dates that were not found in the data set (FALSE), copy, and appended the missing dates to the end of the data set 
* Set the HPCP values for those days to 0 as no rain was observed  
* Re-sort the dataset from oldest to newest observation and save output

### References

[Backfilling missing dates](https://www.excelforum.com/excel-formulas-and-functions/489870-solved-filling-in-missing-dates.html)

[Resolving `ValueWarning`](https://stackoverflow.com/questions/58510659/error-valuewarning-a-date-index-has-been-provided-but-it-has-no-associated-fr)


