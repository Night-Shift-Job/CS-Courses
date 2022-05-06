## optimize::optimize
入口，传入ast根；
- 根节点接受一个hintProcessor，处理传入的hint，通过node::enter方法向qaNameMap写入offset；（TODO: hint）
- 从planBuilderPool中获取一个builder（TODO: golang sync.Pool)
  - do some sctx, buider setup, check privilege
- 判断使用哪个优化器之后，我们进入cascades

## cascades::optimize::FindBestPlan
1. preprocessing$onPhasePreprocessing:
    - 进行一定有效的heuristic处理，注释中提到列剪枝。
    - 调用LogicalPlan.PruneColumns
        - 具体实现在planner/core/rule_column_pruning中

        SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDesc FROM Orders INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
Projection -> Selection -> InnerJoin

- parentUsedCols