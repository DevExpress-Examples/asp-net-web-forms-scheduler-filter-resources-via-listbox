<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128546863/15.2.4%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/E3783)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
<!-- default badges end -->

# Scheduler for ASP.NET Web Forms - How to use ASPxListBox to filter resources

This example illustrates how to use a checked box list to implement resource filtering at the datasource level. The checked box list is implemented by the [ASPxListBox](https://docs.devexpress.com/AspNet/DevExpress.Web.ASPxListBox) control with the [SelectionMode](https://docs.devexpress.com/AspNet/DevExpress.Web.ASPxListBox.SelectionMode) property set to `CheckColumn`. The list box is bound to a resource data source. When the list box selection changes, the scheduler sends a custom `FLTRES` [callback command](https://docs.devexpress.com/AspNet/5462/components/scheduler/concepts/callback-commands).

```js
scheduler.RaiseCallback('FLTRES|' + s.GetSelectedValues().join(','));
```

The command is handled on the server in the `Page_Init` event handler.

```cs
protected void Page_Init(object sender, EventArgs e) {
    string param = Page.Request["__CALLBACKPARAM"]; 
    if (!IsPostBack)
        Session["resourcesSelectCommand"] = defaultResourcesSelectCommand; 
    if (!string.IsNullOrEmpty(param) && param.Contains("FLTRES|")) {
        string resources = param.Substring(param.IndexOf('|') + 1);
        if (resources.Length > 0)
            Session["resourcesSelectCommand"] = string.Format("SELECT [ID], [Model] FROM [Cars] WHERE [ID] IN ({0})", resources);
        else
            Session["resourcesSelectCommand"] = defaultResourcesSelectCommand;
    } 
    SqlDataSourceResources.SelectCommand = Session["resourcesSelectCommand"].ToString();
}
```

That is, we parse callback command parameters and update the resource datasource SQL select command accordingly.

To run this example, You should register a **CarsXtraScheduling** database in your local SQL Server instance. You can download the corresponding SQL scripts from the following example: [How to bind a scheduler to a MS SQL Server database](https://github.com/DevExpress-Examples/asp-net-web-forms-scheduler-bind-to-sql).


