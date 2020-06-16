# BudgetServiceInterface

`BudgetServiceInterface` predefines methods to handle the user budget.

## isBudgetExceeded

This method checks if the budget for user (by user ID) is exceeded.
Throws `BudgetExceededException` if user budget is exceeded.

## getBudgetDetails

Returns a list with user budgets, for example:

```
array(
    'budget_order' => 100.00,
    'budget_month' => 400.00
)
```
 
## getOrderSumOfLastMonth

Returns the sum of the user orders of the last month.

## OrderBudgetService

`OrderBudgetService` is the concrete implementation of `BudgetServiceInterface`.
It checks the user budget that is set in the content. Budget per order and budget per month are considered here.

Service ID: `siso_customer_center.budget_service.order`
