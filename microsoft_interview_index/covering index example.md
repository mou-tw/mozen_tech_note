## 示例场景：员工表查询

假设有一张 `Employees` 表，结构如下：
```
CREATE TABLE Employees (
  EmployeeID   INT PRIMARY KEY,
  Salary       INT,
  [Name]       NVARCHAR(100),
  DepartmentID INT
);
```

现在执行的查询是：
```
SELECT EmployeeID, [Name]
FROM Employees
WHERE Salary = 90000;
```
當無索引時，查询会触发 **Table Scan**，扫描整个表，逐行检验条件，性能较差。


## 添加non cluster index但非覆盖：

```
CREATE INDEX IX_Employees_Salary
  ON Employees (Salary);
```

查询计划可能为 **Index Seek + Key Lookup**：
- 在索引中找到符合 Salary 的行。
- 然后回表（KEY LOOKUP）获取 `Name` , EmployeeID字段，产生额外 I/O 开销。

## 添加覆盖索引（Covering Index):
```
CREATE INDEX IX_Employees_Salary_Name
  ON Employees (Salary)
  INCLUDE ([Name], EmployeeID);
```

- 这个索引在叶级包含了 `Salary`（键列）和被 `SELECT` 的列 (`EmployeeID`, `Name`)。
- 查询可以通过 **Index Only Scan（或 Index Seek）** 完全从索引中获取所有数据，无需回表查找，显著减少磁盘 I/O。


## 總結
- **定义**：非聚集索引中包含了查询所需的所有列（键列 + SELECT/WHERE/ORDER BY 所有列）。
- **优化效果**：
    - 避免 Key Lookup 或 Table Scan。
    - 大幅降低 I/O、减少延迟，特别适用读多写少的场景。
- **注意事项**：
    - 虽然覆盖索引加快读取，但会增加索引大小，带来额外维护开销。
    - 应基于实际查询频率和性能需求进行权衡设计。