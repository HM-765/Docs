# .Net Core 3.1 Cookie 사용하기.
* 우선 Controller class 생성자에 다음과 같이 IHttpContextAccessor를 추가한다.

```C#
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;
    private readonly IHttpContextAccessor _contextAccessor;     //add this.

    public HomeController(ILogger<HomeController> logger, 
    IHttpContextAccessor httpContextAccessor)                   //add this.
    {

        _logger = logger;

        this._contextAccessor = httpContextAccessor;            //add this.
    }

    ...
}
```

* getCookie 메서드를 작성한다.

```C#
public string getCookie(string name)
{
    if (string.IsNullOrEmpty(name))
    {
        return null;
    }
    try
    {
    	IRequestCookieCollection cookies = Request.Cookies;
        if (cookies == null) return null;
        foreach(var cookie in cookies)
        {
            if (!name.Equals(cookie.Key)) continue;
            return cookie.Value;
        }
	}
    catch (Exception ex)
    {

    }
    return null;
}
```