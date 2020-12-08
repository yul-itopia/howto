# How to Execute a Single SQL Query

## Overview

A simple ad hoc way to execute a sql query statement

### 1. Fetch ``` connection ```  from ``` context ```
  - ``` CommandContext::getConnection(dbName) ```

### 2. Prepare SQL Statement
  - Prepare SQL Query Text: ``` sqlText ```
  - Check if the query has been already cached in the connection
    - ``` connection.isPreparedStatementCached(sqlText) ```
    - If not, prepare the query ```sqlText``` in the ``` connection ```
    - The methods of ``` SqlConnection ``` to call: 
        ``` 
        SqlAdHocStatement createAdHocStatement(
            String sql, 
            String[] columns, 
            int[] parameterTypes, 
            boolean query); 
        ``` 
        For Example: 
        ``` 
        SqlAdHocStatement sqlStatement =
             connection.createAdHocStatement(
                 sqlText, 
                 SELECT_COLUMNS_MESSAGING, 
                 new int[] { Types.CHAR }, 
                 true); 
        ```
        Prepare: 
        ``` 
        void prepareAdHocStatement(
                String statementName, 
                SqlAdHocStatement adHocStatement) 
                        throws TSqlException;
        ```
        For Example: 
        ``` 
        connection.prepareAdHocStatement(sqlText, sqlStatement);
        ```
    - Specify ``` SqlExecutionOptions ```
        ``` 
        SqlExecutionOptions execOptions = new SqlExecutionOptions(); 
        execOptions.setMaxRows(maxRowCount);
        ```
### 3. Execute the Query
```
connection.executePreparedStatement(
    sqlText, new Object[] { viewName }, execOptions);
```

## Other Solutions
- ``` MetaSqlSelectBuilder ```
- ``` MetaDynamicSqlHelper ``` 