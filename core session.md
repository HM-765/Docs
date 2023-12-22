# .Net Core 3.1 MVC 프로젝트에서 Session 사용하기.
* ㅇ
## Startup.cs 설정.
Startup.cs 파일의 ConfigureServices, Configure 메소드를 수정한다.
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddMvc().AddSessionStateTempDataProvider(); //add this.
    services.AddSession();                               //add this.
}
```
```C#
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...
    app.UseRouting();
    app.UseSession();       //add this.
    app.UseAuthorization(); //add this.
    ...
}
```
## cshtml 설정.
* model 과 controller를 수정하지 않는 경우는 아래와 같이 cshtml만 수정하여 사용한다.
```html
@using Microsoft.AspNetCore.Http
...
<p> 
    session id: @Context.Session.id
</p>
...
```
* model과 controller를 수정하는 경우는 아래와 같이 수정한다.
```html
...
<p>
    session id : @Model.id
</p>
...
```

## Model 생성 및 Controller 수정
* 모델 생성
```C#
public class SessionViewModel
{

	public string id { get; set; }

}
```
* controller 수정
```C#
using Microsoft.AspNetCore.Http;        //add this.

...

public IActionResult SessionRun()
{
    var session = new SessionViewModel();
    session.id = HttpContext.Session.Id;

	return View(session);
}
```