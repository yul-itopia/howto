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
  - Enter Action Name: ``` Update Invoices ```
  - Click ``` Continue ```
  - Select ``` Literal Key ``` : ``` update_invc ```
  - Select ``` Operation Name ``` : ``` update ```
  - Select ``` Purpose ``` : ``` Task ```
  - Click ``` Submit ```
  - From ``` Admin Console ```, Click ``` Refresh Metadata ```

### Create a new Task View
  - Go to the resource on which the new task action will run: ``` ice_client ```
  - Click the "Personalize" button
  - Click the "Save As" button
    - Enter a New Title: ``` Update Invoices ```
    - Enter a new View Name: ``` ice_client.update_invoices ```
    - Select ``` Task ``` as the new ``` Action ``` 

## Code Preparation