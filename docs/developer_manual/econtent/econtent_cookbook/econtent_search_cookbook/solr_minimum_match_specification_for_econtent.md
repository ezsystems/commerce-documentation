#  Solr Minimum Match specification for econtent 

### The `mm` (Minimum Should Match) Parameter

The table below explains the various ways that mm values can be specified.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
<p>Syntax</p>
</th>
<th><div class="tablesorter-header-inner">
<p>Example</p>
</th>
<th><div class="tablesorter-header-inner">
<p>Description</p>
</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>Positive integer</p></td>
<td><p>3</p></td>
<td><p>Defines the minimum number of clauses that must match, regardless of how many clauses there are in total.</p></td>
</tr>
<tr>
<td><p>Negative integer</p></td>
<td><p>-2</p></td>
<td><p>Sets the minimum number of matching clauses to the total number of optional clauses, minus this value.</p></td>
</tr>
<tr>
<td><p>Percentage</p></td>
<td><p>75%</p></td>
<td><p>Sets the minimum number of matching clauses to this percentage of the total number of optional clauses. <strong>The number computed from the percentage is rounded down and used as the minimum.</strong></p></td>
</tr>
<tr>
<td><p>Negative percentage</p></td>
<td><p>-25%</p></td>
<td><p>Indicates that this percent of the total number of optional clauses can be missing. The number computed from the percentage is rounded down, before being subtracted from the total to determine the minimum number.</p></td>
</tr>
<tr>
<td><p>An expression beginning with a positive integer followed by a &gt; or &lt; sign and another value</p></td>
<td><p>3&lt;90%</p></td>
<td><p>Defines a conditional expression indicating that if the number of optional clauses is equal to (or less than) the integer, they are all required, but if it's greater than the integer, the specification applies. In this example: if there are 1 to 3 clauses they are all required, but for 4 or more clauses only 90% are required.</p></td>
</tr>
<tr>
<td><p>Multiple conditional expressions involving &gt; or &lt; signs</p></td>
<td><p>2&lt;-25% 9&lt;-3</p></td>
<td><p>Defines multiple conditions, each one being valid only for numbers greater than the one before it. In the example at left, if there are 1 or 2 clauses, then both are required. If there are 3-9 clauses all but 25% are required. If there are more then 9 clauses, all but three are required.</p></td>
</tr>
</tbody>
</table>

When specifying `mm` values, keep in mind the following:

  - When dealing with percentages, negative values can be used to get different behavior in edge cases. 75% and -25% mean the same thing when dealing with 4 clauses, but when dealing with 5 clauses 75% means 3 are required, but -25% means 4 are required.
  - If the calculations based on the parameter arguments determine that no optional clauses are needed, the usual rules about Boolean queries still apply at search time. (That is, a Boolean query containing no required clauses must still match at least one optional clause).
  - No matter what number the calculation arrives at, Solr will never use a value greater than the number of optional clauses, or a value less than 1. In other words, no matter how low or how high the calculated result, the minimum number of required matches will never be less than 1 or greater than the number of clauses.
  - When searching across multiple fields that are configured with different query analyzers, the number of optional clauses may differ between the fields. In such a case, the value specified by mm applies to the maximum number of optional clauses. For example, if a query clause is treated as stopword for one of the fields, the number of optional clauses for that field will be smaller than for the other fields. A query with such a stopword clause would not return a match in that field if mm is set to 100% because the removed clause does not count as matched.

Currently mm only works for econtent search and can be specified with the following parameter:

``` 
#Solr minimun match specification string
siso_search.default.min_match_specification_string: '3<80%'
```
