# BudgetServiceInterface

## Introduction

The purpose of this interface is to predefine methods to handle the user budget.

## BudgetServiceInterface

### public function isBudgetExceeded($amount, $userId);

This function checks if the budget for user (by user id) is exceeded.
Throws BudgetExceededException if user budget was exceeded.

### public function getBudgetDetails($userId);

Returns a list with user budgets.
Example:

```
array(
    'budget_order' => 100.00,
    'budget_month' => 400.00
)
```
 
### public function getOrderSumOfLastMonth($userId);

Returns the sum of the user orders of the last month.
