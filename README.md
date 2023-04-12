<!-- default badges list -->
<!-- default badges end -->
## DataGrid for .NET MAUI - Kanban View

This example demonstrates how to implement a Kanban-like view with our [Data Grid](https://docs.devexpress.com/MAUI/403255/data-grid/data-grid). In this solution, the [Data Grid](https://docs.devexpress.com/MAUI/403255/data-grid/data-grid) contains multiple groups (Planned, Coding, Testing, Done) with items. You can drag a [Data Grid](https://docs.devexpress.com/MAUI/403255/data-grid/data-grid) item from a group and drop it into another group.

<img src="https://user-images.githubusercontent.com/12169834/231455223-959dfe6c-7d7e-465b-8814-d48fdcc7ad55.png" width="30%"/>


Included control and its properties:

* [DataGridView](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView): [IsColumnHeaderVisible](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.IsColumnHeaderVisible), [AllowDragDropRows](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.AllowDragDropRows), [AllowDragDropSortedRows](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.AllowDragDropSortedRows), [AllowGroupCollapse](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.AllowGroupCollapse), [CustomSort](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.CustomSort), [CustomGroup](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.CustomGroup), [CompleteRowDragDrop](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.CompleteRowDragDrop), [DragRow](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.DragRow), [GroupRowAppearance](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.GroupRowAppearance), [CellAppearance](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.CellAppearance), [GroupRowTemplate](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.GroupRowTemplate), [TemplateColumn](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.TemplateColumn)

## Implementation Details


* You can handle the [DataGridView.CustomSort](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.CustomSort) event to sort groups in the order you want:

    ```csharp
    private void DataGridView_CustomSort(object sender, DevExpress.Maui.DataGrid.CustomSortEventArgs e) {
        if (e.Column.FieldName == "Stage")
            e.Result = Comparer.Default.Compare(viewModel.StageOrder[(string)e.Value1], viewModel.StageOrder[(string)e.Value2]);
    }
    ```
    File to Look At: [MainPage.xaml.cs](CS/MainPage.xaml.cs)

* If a [Data Grid](https://docs.devexpress.com/MAUI/403255/data-grid/data-grid) group does not contain any item, the placeholder (based on the [Border](https://learn.microsoft.com/en-us/dotnet/maui/user-interface/controls/border) control) is displayed. To achive this, we handle the [CompleteRowDragDrop](https://docs.devexpress.com/MAUI/DevExpress.Maui.DataGrid.DataGridView.CompleteRowDragDrop) event and switch the `IsPlaceholder` property. The placeholder is displayed when the `IsPlaceholder` property is `true`. .NET MAUI [triggers](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/triggers) allows you to conditionally display the specific element:

    To control the visibility of an empty group's placeholder we used . 

    ```xaml
    <dxg:TemplateColumn FieldName="Title">
        <dxg:TemplateColumn.DisplayTemplate>
            <DataTemplate>
                <Grid>
                    <Border ...>
                        <Border.Triggers>
                            <DataTrigger Binding="{Binding Item.IsPlaceholder}" Value="False" TargetType="Border">
                                <Setter Property="IsVisible" Value="True"/>
                            </DataTrigger>
                        </Border.Triggers>
                    </Border>
                    <Border IsVisible="False" BackgroundColor="Transparent">
                        <Label Text="Drag Items here" Margin="10,20,10,20" HorizontalOptions="Center"/>
                        <Border.Triggers>
                            <DataTrigger Binding="{Binding Item.IsPlaceholder}" Value="True" TargetType="Border">
                                <Setter Property="IsVisible" Value="True"/>
                            </DataTrigger>
                        </Border.Triggers>
                    </Border>
                </Grid>
            </DataTemplate>
        </dxg:TemplateColumn.DisplayTemplate>
    </dxg:TemplateColumn>
    ```

    File to Look At: [MainPage.xaml](CS/MainPage.xaml)

    ```csharp
    private void DataGridView_CompleteRowDragDrop(object sender, DevExpress.Maui.DataGrid.CompleteRowDragDropEventArgs e) {
        AddPlaceholderTaskToSourceGroup();
        RemovePlaceholderTaskFromTargetGroup();
    }

    void AddPlaceholderTaskToSourceGroup() {
        if (!viewModel.Tasks.Any(t => t.Stage == draggedTaskOriginalStage)) {
            viewModel.Tasks.Add(new TaskToDo() { IsPlaceholder = true, Stage = draggedTaskOriginalStage });
        }
    }

    void RemovePlaceholderTaskFromTargetGroup() {
        string newDraggedTaskStage = draggedItem.Stage;
        TaskToDo stabTask = viewModel.Tasks.FirstOrDefault(t => t.Stage == newDraggedTaskStage && t.IsPlaceholder);
        if (stabTask != null) {
            viewModel.Tasks.Remove(stabTask);
        }
    }
    ```

    File to Look At: [MainPage.xaml.cs](CS/MainPage.xaml.cs)


## Files to Look At

<!-- default file list -->
* [MainPage.xaml](CS/MainPage.xaml)
* [MainPage.xaml.cs](CS/MainPage.xaml.cs)
* [MainViewModel.cs](CS/MainViewModel.cs)
<!-- default file list end -->

## Documentation

* [Featured Scenario: Data Grid - Replicate Kanban View](https://docs.devexpress.com/MAUI/404358)
* [Featured Scenarios](https://docs.devexpress.com/MAUI/404291)
* [Data Grid](https://docs.devexpress.com/MAUI/403255/data-grid/data-grid)

## More Examples

* [DevExpress Mobile UI for .NET MAUI](https://github.com/DevExpress-Examples/maui-demo-app/)
