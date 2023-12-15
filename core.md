.NET Core 3.1 및 이후 버전에서는 IHostedService 인터페이스를 사용하여 비동기 작업을 실행하는 서비스를 만들 수 있습니다. 이것은 IHostedService를 구현하는 클래스를 만들고, Startup.cs 파일에서 등록하여 사용할 수 있습니다.
 
IHostedService는 애플리케이션의 시작 및 종료 시에 비동기적으로 실행될 수 있는 서비스를 정의하는 인터페이스입니다. IHostedService를 구현하는 클래스는 StartAsync 및 StopAsync 메서드를 제공해야 합니다.
 
IHostedService를 사용하면 비동기적인 작업을 주기적으로 수행하는 것이 가능합니다. 주기적인 작업을 실행하기 위해서는 System.Threading.Timer 클래스를 활용하면 됩니다. IHostedService를 구현한 클래스에서 Timer를 이용하여 일정 시간마다 실행되는 작업을 정의할 수 있습니다.

```C#
using Microsoft.Extensions.Hosting;
using System;
using System.Threading;
using System.Threading.Tasks;

public class MyHostedService : IHostedService, IDisposable
{
    private Timer _timer;

    public Task StartAsync(CancellationToken cancellationToken)
    {
        // TimerCallback을 통해 주기적으로 실행될 메서드를 지정
        _timer = new Timer(DoWork, null, TimeSpan.Zero, TimeSpan.FromSeconds(10));

        return Task.CompletedTask;
    }

    private void DoWork(object state)
    {
        Console.WriteLine("MyHostedService is doing some work...");
        // 주기적으로 실행될 작업을 여기에 추가
    }

    public Task StopAsync(CancellationToken cancellationToken)
    {
        // Timer를 정리
        _timer?.Change(Timeout.Infinite, 0);
        return Task.CompletedTask;
    }

    public void Dispose()
    {
        // Timer를 정리
        _timer?.Dispose();
    }
}
```

위의 예제에서 DoWork 메서드는 주기적으로 실행될 작업을 정의합니다. Timer는 StartAsync 메서드에서 초기화되며, StopAsync 메서드에서 정리됩니다. 이렇게 구현된 IHostedService를 서비스로 등록하면 애플리케이션이 실행되는 동안 주기적으로 작업이 수행됩니다.

계속 실행되어야 하는 작업을 위한 BackgroundService in .NET Core
* https://jettstream.tistory.com/111

IHostedService 및 BackgroundService 클래스를 사용하여 마이크로 서비스에서 백그라운드 작업 구현
* https://learn.microsoft.com/ko-kr/dotnet/architecture/microservices/multi-container-microservice-net-applications/background-tasks-with-ihostedservice

Background tasks with hosted services in ASP.NET Core
* https://learn.microsoft.com/en-us/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-8.0&tabs=netcore-cli