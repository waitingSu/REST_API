## C# MVC How to register nest resource in REST API 

  Some people don't want to use {action} in Web AP,because they carry out this REST style with perseverance.
so there are some  some ways to achieve.

In WebApiConfig :
```markdown
 public static class WebApiConfig
 {
    public static void Register(HttpConfiguration config)
    {
      config.Routes.MapHttpRoute(
        name: "DefaultApi",
        routeTemplate: "api/{controller}/{id}",
        defaults: new { id = RouteParameter.Optional }
      );
      
      config.Routes.MapHttpRoute(
      name: "OneLevelNested",
      routeTemplate: "api/Companies/{companyId}/departments/{id}",
      constraints: new { httpMethod = new HttpMethodConstraint(new string[] { "GET" }) },
      defaults: new { controller = "Companies", action = "GetDepartments", id = RouteParameter.Optional }
      );
    }      
 }
```
In CompaniesController :
```markdown
// api/Companies/1
public ResultInfo Get(string id)
{
  //Get company info
}

// api/Companies/1/departments/12
public ResultInfo GetDepartments(string companyId, string id)
{
  //Get GetDepartments for the companyId
}
```

But this is still not perfect solution,you will need different routeConfig to get defferent methods.

The clean REST style soultion should use :
```markdown
api/Departments/12?companyId=1
```
In DepartmentsController :
```markdown
// api/Departments/12?companyId=1
public ResultInfo Get(string id,string companyId)
{
  //Get GetDepartments for the companyId
}
```
