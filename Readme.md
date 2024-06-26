<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128546863/15.2.4%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/E3783)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
[![](https://img.shields.io/badge/💬_Leave_Feedback-feecdd?style=flat-square)](#does-this-example-address-your-development-requirementsobjectives)
<!-- default badges end -->

# Scheduler for ASP.NET Web Forms - How to use ASPxListBox to filter resources

This example illustrates how to use a check box list to implement resource filtering at the data source level. The check box list is implemented by the [ASPxListBox](https://docs.devexpress.com/AspNet/DevExpress.Web.ASPxListBox) control with the [SelectionMode](https://docs.devexpress.com/AspNet/DevExpress.Web.ASPxListBox.SelectionMode) property set to `CheckColumn`. The list box is bound to a resource data source. When the list box selection changes, the scheduler sends a custom `FLTRES` [callback command](https://docs.devexpress.com/AspNet/5462/components/scheduler/concepts/callback-commands).

```aspx
<dxe:ASPxListBox ID="lbResources" ...>
    <ClientSideEvents SelectedIndexChanged="function(s, e) { scheduler.RaiseCallback('FLTRES|' + s.GetSelectedValues().join(',')); }" />
</dxe:ASPxListBox>
```

The command is handled on the server on the `Page_Init` event.

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

To run this example, you should register the **CarsXtraScheduling** database in your local SQL Server instance. You can download the corresponding SQL scripts from the following example: [How to bind a scheduler to an MS SQL Server database](https://github.com/DevExpress-Examples/asp-net-web-forms-scheduler-bind-to-sql).

## Files to Review

* [Default.aspx](./CS/WebSite/Default.aspx) (VB: [Default.aspx](./VB/WebSite/Default.aspx))
* [Default.aspx.cs](./CS/WebSite/Default.aspx.cs) (VB: [Default.aspx.vb](./VB/WebSite/Default.aspx.vb))
<!-- feedback -->
## Does this example address your development requirements/objectives?

[<img src="https://www.devexpress.com/support/examples/i/yes-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=asp-net-web-forms-scheduler-filter-resources-via-listbox&~~~was_helpful=yes) [<img src="https://www.devexpress.com/support/examples/i/no-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=asp-net-web-forms-scheduler-filter-resources-via-listbox&~~~was_helpful=no)

(you will be redirected to DevExpress.com to submit your response)
<!-- feedback end -->
