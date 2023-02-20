# Data Table

Datatable is a component for rendering tables based on AG-grid Community.

Specifics:

- Does not do queries under the hood. Instead, receives a function to do so: `doQuery: (params: { pageSize: number; pageNumber: number; search: string; }) => ...`;
- Supports eager or lazy loading;
- Configurable page size;
- onRowClicked handler;
- supports making column data clickable with optional start or end icons, row expanding.

Notes:

- Row expanding is a premium feature itself. To overcome that, in this component it is implement as rendering a new AG-grid table instead of the expandable row.
