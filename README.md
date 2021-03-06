# Ember Table

An addon to support large data set and a number of features around table. `Ember Table` can
handle over 100,000 rows without and rendering or performance issue. This version of `Ember Table`
supports Ember 1.11 to latest version of Ember.

## Install

```bash
ember install ember-table
```

## Features
- Column resizing, column reordering.
- Table resizing.
- Fixed first column.
- Custom row and custom header.
- Handles transient state at cell level.
- Single, multiple row selection.
- Table grouping.

## Usage.

To use `Ember Table`, you need to create `columns` and `rows` dataset.

`columns` is an array of objects which has multiple fields to define behavior of column.
Two required field in each object is `columnName` and `valuePath`.

```javascript
  columns: [
    {
      columnName: `Open time`,
      valuePath: `open`
    },
    {
      columnName: `Close time`,
      valuePath: `close`
    }
  ]
```

`rows` could be a javascript array, ember array or any data structure that implements `length` and
`objectAt(index)`. This flexibity gives application to avoid having all data at front but loads more
data as user scrolls. Each object in the `rows` data structure should contains all fields defined
by all `valuePath` in `columns` array.

```javascript
  rows: computed(function() {
    const rows = emberA();

    rows.pushObject({
      open: '8AM',
      close: '8PM'
    });

    rows.pushObject({
      open: '11AM',
      close: '9PM'
    });

    return rows;
  })
```

### Template

The following template renders a simple table.

```
  {{#ember-table
    rows=rows
    columns=columns
    as |row|
  }}
    {{#ember-table-row
      row=row
      as |cell|
    }}
      {{cell.value}}
    {{/ember-table-row}}
  {{/ember-table}}
```

To have your own custom cell, pass in the cell component name in each element of `columns` array and
specify it in the template.

```
  {{#ember-table
    rows=rows
    columns=columns
    as |row|
  }}
    {{#ember-table-row
      row=row
      as |cell|
    }}
      {{#component cell.column.cellComponent
        cell=cell
      }}
    {{/ember-table-row}}
  {{/ember-table}}
```

The rendered table is a plain table without any styling. You can define styling for your own table.
If you want to use default table style, import the `ember-table/default` SASS file.

### Optional Footer
To use footer for your table, pass `footerRows` param to ember table. Each element in `footerRows`
represents a row in table footer. The footer row takes `valuePath` field in each column to render
data for each footer cell, similar to table body.

### Custom header and custom footer
By default Ember table header renders text defined by `columnName` or `footerValue` inside each
`column`. To customize table header or footer, you can pass in `headerComponent` and
`footerComponent` fields in each column data. When the `headerComponent`(or `footerComponent`) is
defined, the `columnName`(or `footerValue`) field is ignored.

#### Usage
```
{
  headerComponent: 'complex-header',
  footerComponent: 'custom-footer',
  valuePath: 'url',
  width: 180
}
```

## Migrating from old Ember table
To support smooth migration from old version of Ember table (support only till ember 1.11), we have
move the old source code to separate package [ember-table-legacy](https://github.com/Addepar/ember-table-legacy).
It's a separate package from this Ember table package and you can install it using yarn or npm.
This allows you to have 2 versions of ember table in your code base and you can start your migrating
one table at at time. The recommended migration steps are as follows (if you are using ember 1.11):

1) Rename all your ember-table import to ember-table-legacy. (for example:
`import EmberTable from 'ember-table/components/ember-table'` becomes
`import EmberTableLegacy from 'ember-table-legacy/components/ember-table-legacy'`. Remove reference
of `ember-table` in `package.json`.
2) Install `ember-table-legacy` using `yarn add ember-table-legacy` or `npm install ember-table-legacy`
3) Run your app to make sure that it works without issue.
4) Reinstall the latest version of this `ember-table` repo.
5) You can start using new version of Ember table from now or replacing the old ones.
