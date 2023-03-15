# Data Table

## Install

```
npm i @eten-lab/data-table
```

## Description

Datatable is a component for rendering tables based on AG-grid Community.

Features:

- Does not do queries under the hood. Instead, receives a function to do so: `doQuery: (params: { pageSize: number; pageNumber: number; search: string; }) => ...`;
- Supports both eager and lazy loading;
- Configurable page size;
- onRowClicked handler for a whole row;
- supports making column data clickable with optional start or end icons, row expanding.

Notes:

- Row expanding is a premium feature itself. To overcome that, it is implement as rendering a new AG-grid table instead of the expandable row. When a row is expanded, resizing the table will end up in inconsistent columns' width.
