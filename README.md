# How to enable cell editing with Enter in Flutter DataTable (SfDataGrid).

In this article, we will show you how to enable cell editing with Enter in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

STEP 1: Create a custom selection manager class by extending [RowSelectionManager](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/RowSelectionManager-class.html). In the handleKeyEvent method, check if the Enter key is pressed using keyEvent. Then, call [DataGridController.beginEdit](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridController/beginEdit.html) inside WidgetsBinding.instance.addPostFrameCallback, using [DataGridController.currentCell](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridController/currentCell.html).

```dart
class CustomSelectionManager extends RowSelectionManager {
  CustomSelectionManager(this.dataGridController);
  DataGridController dataGridController;

  @override
  Future<void> handleKeyEvent(KeyEvent keyEvent) async {
    if (keyEvent.logicalKey == LogicalKeyboardKey.enter) {
      WidgetsBinding.instance.addPostFrameCallback((timeStamp) {
        dataGridController.beginEdit(dataGridController.currentCell);
      });
    }
    super.handleKeyEvent(keyEvent);
  }
}
```

STEP 2: Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the required properties. Create an instance of the CustomSelectionManager class and assign it to the [SfDataGrid.selectionManager](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/selectionManager.html) property. Create an instance of the DataGridController and assign it to the [SfDataGrid.controller](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/controller.html) property.

```dart
List<Employee> _employees = <Employee>[];
  late EmployeeDataSource _employeeDataSource;
  DataGridController dataGridController = DataGridController();
  late CustomSelectionManager customSelectionManager;

  @override
  void initState() {
    super.initState();
    _employees = getEmployeeData();
    _employeeDataSource = EmployeeDataSource(_employees, context);
    customSelectionManager = CustomSelectionManager(dataGridController);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Syncfusion Flutter DataGrid')),
      body: SfDataGrid(
        source: _employeeDataSource,
        controller: dataGridController,
        allowEditing: true,
        selectionManager: customSelectionManager,
        navigationMode: GridNavigationMode.cell,
        selectionMode: SelectionMode.single,
        columns: <GridColumn>[
          GridColumn(
            columnName: 'id',
            label: Container(
              padding: const EdgeInsets.all(8.0),
              alignment: Alignment.centerRight,
              child: const Text('ID'),
            ),
          ),
          GridColumn(
            columnName: 'name',
            label: Container(
              padding: const EdgeInsets.all(8.0),
              alignment: Alignment.centerLeft,
              child: const Text('Name'),
            ),
          ),
          GridColumn(
            allowEditing: false,
            columnName: 'designation',
            label: Container(
              padding: const EdgeInsets.all(8.0),
              alignment: Alignment.centerLeft,
              child: const Text('Designation', overflow: TextOverflow.ellipsis),
            ),
          ),
          GridColumn(
            columnName: 'salary',
            label: Container(
              padding: const EdgeInsets.all(8.0),
              alignment: Alignment.centerRight,
              child: const Text('Salary'),
            ),
          ),
        ],
        columnWidthMode: ColumnWidthMode.fill,
      ),
    );
  }
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-enable-cell-editing-with-Enter-in-Flutter-DataTable).