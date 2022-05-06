# OperationTransformer
**Parse tree to logical operator tree transformation**.
- Start from unittest:
  - [x] SelectStatementSimpleTest
  - [x] SelectStatementAggregateTest

They first convert parse result(statement) into operator tree with `operatorTransformer::ConvertToOpExpression`; 
Inside it, we call  
`void QueryToOperatorTransformer::Visit(common::ManagedPointer<parser::SelectStatement> op)` to iterate over each expr and fill attr in each tree node.

For example, it convert `SELECT MAX(b1) FROM B GROUP BY b2` into "{\"Op\":\"LogicalAggregateAndGroupBy\",\"Children\":"
      "[{\"Op\":\"LogicalGet\",}]}";

# Optimize
1. Start from unittest: DeliveryGetCustomerId, which is a query with predication;
1. push everything [TopDownRewrite, BottomUpRewrite, OptimizeGroup, DeriveStats] into taskStack, and `ExecuteTaskStack`;
2. 