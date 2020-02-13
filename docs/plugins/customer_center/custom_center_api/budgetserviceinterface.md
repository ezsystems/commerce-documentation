# BudgetServiceInterface 

## Introduction

The purpose of this interface is to predefine methods to handle the user budget.

## BudgetServiceInterface

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Method
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>public function isBudgetExceeded($amount, $userId);</code></pre></td>
<td><pre><code>This function checks if the budget for user (by user id) is exceeded.</code></pre>
<pre><code>Throws BudgetExceededException if user budget was exceeded.</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>public function getBudgetDetails($userId);</code></pre></td>
<td><pre><code>Returns a list with user budgets.</code></pre>
<pre><code>Example:
 array(
 &#39;budget_order&#39; =&gt; 100.00,
 &#39;budget_month&#39; =&gt; 400.00
 )</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>public function getOrderSumOfLastMonth($userId);</code></pre></td>
<td><pre><code>Returns the sum of the user orders of the last month.</code></pre></td>
</tr>
</tbody>
</table>
