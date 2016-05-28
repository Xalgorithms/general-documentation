### Table Transformation in the Limited-Purpose "Xalgo" Language
In the XA system there is a requirement for [Repository](https://github.com/Xalgorithms/xa-repository.simple) owners to be able to represent rules in a readable format that can be stored in the [Registry](https://github.com/Xalgorithms/xa-registry). These rules are composed of tables and series of operations on tables. The "Xalgo" language documented in the following sections fulfills the specified *series of operations* component of this thinking. It is intentionally very limited in scope.

The language itself is *line-oriented*. Each line in a program represents a single, atomic operation on a table or the context in which the program is run. It includes the ability to specific *expectations* and *outputs* of the program. There are two primary data structures in the language:

* **tables**: All data in or supplied to the program is in the form of tables referenced by a name. These tables are supplied either by the calling context **or** as retrievals within the program. Once assigned to a *name* the name *cannot* be reassigned. Tables are represented with a JSON-like syntax.
* **stack**: Tables to be mutated are placed on the stack. Any results of a mutation is pushed onto the stack.

### Operations
#### Tables
```ATTACH <url> AS <name>```

Associate a *repository* at the URL with a name within the scope of the current program.

```PULL <repo>:<rule>:<version> AS <name>[<c0>, ..., <cN>]```

Pull a table with the given *name* and *version* from a previously named repository. The table will be referencable using the supplied *name*.

```INVOKE <repo>:<rule>:<version>```

Pull a rule with the given *name* and *version* from a previously named repository. The rule is executed with a calling context which includes **all named tables** from the current context. The executed rule will have a clean stack. Any commits from the executed rule will become availble in the current context.

```EXPECTS <name>[<c0>, ..., <cN>]```

The program expects the calling context of the interpreter to supply a table with the specified *name* and *column names*.

```COMMIT <name>[<c0, ..., <cN>]```

Attach the top of the stack to *named table* in the output to the calling context. Column names can be optionally specified.

#### Stack
```
PUSH <name>
```

Push a *named table* onto the stack.

```
POP
```

Pop the stack. The table that was on the top of the stack is lost.

```
DUPLICATE
```

Duplicate the top of the stack.

#### Mutations

```
JOIN USING [[l0, ..., lN], [r0, ..., rN]] INCLUDE [c0, ..., cN]
```

Join two tables where the columns from the left match the columns on the right. The tables are pulled from the stack as right, then left. The resulting table is pushed to the stack. The INCLUDE is optional and specifies a list of columns from the right table that should appear in the result. If there is no INCLUDE, all columns are merged into the resulting table. If there are column name collisions, the columns from the right table take precedence. This can be used to replace columns in the left table.

*More mutations will be added as required*.

### Examples

This is a generic example to show the way that JOIN will function.

##### Tables

```
foo = [
 { a: 1, b: 2, c: 3 },
 { a: 2, b: 4, c: 6 },
 { a: 3, b: 6, c: 9 },
]

bar = [
  { x: 2, y: 6 },
  { x: 3, y: 12 },
]
```

##### Program

```
EXPECTS foo[a, b, c]
EXPECTS bar[x, y]
PUSH foo
PUSH bar
JOIN USING [[a, b], [x, y]]
COMMIT t[a, y]
```

This program yields:

```
t = [
 # no matches where x=1
 { a: 1 },
 { a: 2, y: 6 },
 { a: 3, y: 12 },
]
```

This example shows how a product code (SKU) might be converted to a UNSPSC code.

##### Tables

```
# a collection of items pulled from a cllection of invoices
items = [
  { name: 'Laptop', sku: 'A12345' },
  { name: 'Power Adapter', sku: 'XXX111' },
]

# refer below, this is not provided by lichen - it's pulled from the registry
skus_unspscs = [
  # supplier has coded their laptop SKU as 'consumer electronics'
  { sku: 'A12345', unspsc: '52160000' }
]
```

##### Program

```
# table supplied by lichen
EXPECTS items[name, sku]

# pull external table from the registry
ATTACH http://www.xalgorithms.org AS xa
# syntax indicates that table should at least have two columns
PULL xa:supplier_skus:20160511 AS skus_unspscs[sku, unspsc]

PUSH items
PUSH skus_unspscs
JOIN USING [[sku], [sku]] INCLUDE [unspsc]

# output the result
COMMIT unspsc_items[name, unspsc]
```

Yields:

```
unspsc_items = [
  { name: 'Laptop', unspsc: '52160000' },
  # no unspsc for this item because it was not coded
  # in xa:supplier_skus:20160511
  { name: 'Power Adapter' },
]
```
Dual Licensed: CC-by-4.0 and Apache 2.0 