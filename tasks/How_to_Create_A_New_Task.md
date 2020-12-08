# How to Create a New Task

## Overview

This tutorial demonstrates how to create a task to do a routine job

## Metadata Preparation

### Create a new ``` Table Action ```
  - Log into your portal as ``` tecsys ```
  - Go to resource ``` meta_md_table_action ```
  - Click ``` Create ```
  - Enter Database Name: ``` ice ```
  - Enter Table Name: ``` client ```
  - Enter Action Name: ``` update_invoices ```
  - Click ``` Continue ```
  - Select ``` Literal Key ``` : ``` update_invc ```
  - Select ``` Operation Name ``` : ``` update ```
  - Select ``` Purpose ``` : ``` Task ```
  - Click ``` Submit ```
  - From ``` Admin Console ```, Click ``` Refresh Metadata ```

### Create a new Task View
  - Go to the resource on which the new task action will run: ``` ice_client ```
  - Click the ``` Personalize ``` button
  - Click the ``` Save As ``` button
    - Enter a New Title: ``` Update Invoices ```
    - Enter a new View Name: ``` ice_client.update_invoices ```
    - Select ``` Update Invoices ``` as the new ``` Action ``` 

## Code Preparation
- Package 
    - ``` com.tecsys.ice.ext.api.task ```
### Create a new class to perform the task action
  - Class naming convention 
    - Name Pattern: ``` ApiTask_[Table Name]__[Action Name]__[Operation Name]_[Other Names]```
    - For example: 
      - Table Name: ``` client ```
      - Action Name: ``` update_invoices ```
      - Operation Name: ``` update ```
      - Other Names: ``` invoices ```
      - Class Name: ApiTask_client__update_invoices_update_invoices
### Create a entry class
  - Class naming convention 
    - Name Pattern: ``` ApiTask_[Table Name]__[Action Name] ```
    - For example: 
      - Table Name:  ``` client ```
      - Action Name:  ``` update_invoices ```
      - Class Name: ``` ApiTask_client__update_invoices ```

## Code Example
```
public class ApiTask_v_m_effective_properties__reset_debug_keys 
        extends MetaAbstractTask {
    @Override
    public MetaTaskResponse processTask(MetaTaskRequest request) {
        return processStep(
            ApiTask_v_m_effective_properties__reset_debug_keys__update_key.class,
            request,
            new MetaTaskConfiguration(MetaTaskTransactionLevel.SINGLE_TRANSACTION));
    }
}

public class ApiTask_v_m_effective_properties__reset_debug_keys__update_key 
        extends MetaAbstractTask {

    private static final Literal TASK_TITLE = new Literal("reset_debug_keys_task");
    private static final String TRANSFORMATIONRULEENGINE_LOGMESSAGE = "debug.key.TransformationRuleEngine.LogMessage";

    @Override
    public MetaTaskResponse processTask(MetaTaskRequest request) {

        logUserMessage(request, Literal.createInfo("%1_started", new Object[] { TASK_TITLE }));

        MetaTaskResponse response = new MetaTaskResponse();

        MetaViewEffectivePropertiesSearchRequest searchRequest = MetaViewEffectiveProperties
            .newSearchRequest(request.getTableObjectContext());
        searchRequest.selectAttributeKey();
        searchRequest.selectAttributeValue();
        searchRequest.addSearchForCondition(QbeUtil.buildWildcardInBackExpression("debug.key"));

        MetaViewEffectivePropertiesSearchResults searchResults = searchRequest.search();

        boolean hasDebugKeyEnabled = false;
        for (int i = 0; i < searchResults.size(); i++) {
            MetaViewEffectiveProperties debugKeyProperty = searchResults.instantiateRow(i);

            if (debugKeyProperty.getAttributeKey().contentEquals(TRANSFORMATIONRULEENGINE_LOGMESSAGE)
                || debugKeyProperty.getAttributeValue().equals("0")) {
                continue;
            }

            logUserMessage(
                request,
                Literal
                .createInfo(
                    "%1_turned",
                    new Object[] { debugKeyProperty.getAttributeKey() }));

            debugKeyProperty.setAttributeValue("0");
            debugKeyProperty.modify();
            hasDebugKeyEnabled = true;
        }

        if (!hasDebugKeyEnabled) {
            logUserMessage(request, Literal.createInfo("no_debug_keys_changed"));
        }

        logUserMessage(request, Literal.createInfo("%1_completed_success", new Object[] { TASK_TITLE }));

        return response;
    }
}
```