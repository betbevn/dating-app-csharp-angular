## **ASPDOTNET**: Command line

```bash
dotnet new list: Xem các tính năng của dotnet có thể làm được
dotnet new sln: Tạo một dự án mới theo tên thư mục
dotnet new webapi -n API: Tạo template webapi (Sẽ tạo một thư mục API, trong thư mục sẽ chứa toàn bộ source code)
dotnet sln add API: Thêm template API (solution) vừa tạo vào dự án
dotnet sln list: Liệt kê các solution có trong dự án vừa tạo

dotnet add reference <ProjectReference>: Thêm dự án tham chiếu tới
dotnet add package <PACKAGE_NAME>: Thêm gói vào project
dotnet remove package <PACKAGE_NAME>: Xoá gói khỏi project

dotnet build: Nên build một project dot net core trước khi vào làm, để biết xem đã đủ package hay chưa
dotnet list package: Để xem các package đã được cài đặt
dotnet run

dotnet run -lp https: Chạy với https
dotnet dev-certs https —-clean: Clear hết certificate cũ để tạo một certificate mới
dotnet dev-certs https —trust: Trust certificate vừa tạo

dotnet restore: Để cập nhật lại các package trong file csproj mỗi khi thêm/xoá package

dotnet watch run: Để run code mới nhất, không cần phải chạy lại server mỗi khi code thay đổi (Hot Reload)
dotnet tool list -g: Để xem các package đã được installed

dotnet ef migrations add InitialCreate -o Data/Migrations: Tạo script migration từ Entity
dotnet ef migrations add UserPasswordAdded: Tạo migration từ entities

dotnet ef database update: Để thực thi các script vừa tạo vào trong database
dotnet ef database drop: Xoá tất cả các bảng trong database

```

## **ClaimsPrincipal**: To retrieve user informaion from claims

```bash

# Way 1: Implement extension

public static class ClaimsPrincipalExtensions
{
    public static string GetUserEmail(this ClaimsPrincipal principal)
    {
        return principal.FindFirstValue(ClaimTypes.Email);
    }

    public static string GetUserId(this ClaimsPrincipal principal)
    {
        return principal.FindFirstValue(ClaimTypes.NameIdentifier);
    }

    public static string GetUserName(this ClaimsPrincipal principal)
    {
        return principal.FindFirstValue(ClaimTypes.Name);
    }

    public static bool IsCurrentUser(this ClaimsPrincipal principal, string id)
    {
        var currentUserId = GetUserId(principal);

        return string.Equals(currentUserId, id, StringComparison.OrdinalIgnoreCase);
    }
}


var userId = User.GetUserId();

# Way 2: Using IHttpContextAccessor class

public class QueryHandler : IRequestHandler<Query, Result>
{
    private readonly CloudpressDbContext _dbContext;
    private readonly IHttpContextAccessor _httpContextAccessor;

    public QueryHandler(CloudpressDbContext dbContext, IHttpContextAccessor httpContextAccessor)
    {
        _dbContext = dbContext;
        _httpContextAccessor = httpContextAccessor;
    }

    public async Task<ValidationResult> Handle(Query request, CancellationToken cancellationToken)
    {
        var userId  = _httpContextAccessor.HttpContext.User.GetUserId();

        // do some stuff with the user, for example retrieve the orders for the current user
        var orders = _dbContext.Orders.Where(o => o.UserId == userId);
    }
}

```

## **Truy cập biến môi trường**

```bash
# Way 1:

{
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  },
  "MyKey": "My appsettings.json Value",
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}

public class TestModel : PageModel
{
    // requires using Microsoft.Extensions.Configuration;
    private readonly IConfiguration Configuration;

    public TestModel(IConfiguration configuration)
    {
        Configuration = configuration;
    }

    public ContentResult OnGet()
    {
        var myKeyValue = Configuration["MyKey"];
        var title = Configuration["Position:Title"];
        var name = Configuration["Position:Name"];
        var defaultLogLevel = Configuration["Logging:LogLevel:Default"];


        return Content($"MyKey value: {myKeyValue} \n" +
                       $"Title: {title} \n" +
                       $"Name: {name} \n" +
                       $"Default Log Level: {defaultLogLevel}");
    }
}

# Way 2: IConfiguration configuration

configuration.GetValue<string>("AllowedHosts"); # Lấy giá trị của trường AllowedHosts trong file appsettings.json hoặc appsettings.{enviroment}.json
configuration.GetSection("CloudinarySettings"); # Lấy CloudinarySettings section trong file appsettings.json hoặc appsettings.{enviroment}.json

# Way 3: IOptions

IOptions<CloudinarySettings> config

```

## **Dependency injection**

```bash
IServiceCollection Add{name}Services(this IServiceCollection services, IConfiguration config)
{
  services.AddScoped<ITokenService, TokenService>();
  services.Configure<CloudinarySettings>(config.GetSection("CloudinarySettings")); // IOptions<CloudinarySettings> config
}
```

## **AutoMapper**

```bash
# Implement from Profile class
    public class AutoMapperProfiles : Profile
    {
        public AutoMapperProfiles()
        {
            CreateMap<AppUser, MemberDto>()
            .ForMember(dest => dest.PhotoUrl, opt => opt.MapFrom(src => src.Photos.FirstOrDefault(x => x.IsMain).Url))
            .ForMember(dest => dest.Age, opt => opt.MapFrom(src => src.DateOfBirth.CalculateAge()));
            CreateMap<Photo, PhotoDto>();
            CreateMap<MemberUpdateDto, AppUser>();
            CreateMap<RegisterDto, AppUser>();
        }
    }

```

## **XUnit**

```bash
# Step 1: The dotnet new sln command creates a new solution in the unit-test.

dotnet new sln -o unit-test

# Step 2: Change directory to the unit-test.

cd unit-test

# Step 3: The dotnet new classlib command creates a new class library project in the PrimeService folder. The new class library will contain the code to be tested.

dotnet new classlib -o PrimeService

# Step 4: In the unit-test directory, run the following command to add the class library project to the solution

dotnet sln add ./PrimeService/PrimeService.csproj

# Step 5: Create the PrimeService.Tests project by running the following command:

dotnet new xunit -o PrimeService.Tests

# Step 6: Add the test project to the solution file by running the following command:

dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj

# Step 7: Add the PrimeService class library as a dependency to the PrimeService.Tests project:

dotnet add ./PrimeService.Tests/PrimeService.Tests.csproj reference ./PrimeService/PrimeService.csproj

```

```bash
# [Fact] attribute declares a test method that's run by the test runner
# [Theory] represents a suite of tests that execute the same code but have different input arguments.
# [InlineData] attribute specifies values for those inputs.
```

## **SignalR**

```bash

# Step 1: Create a presence hub and re-implement OnConnectedAsync and OnDisconnectedAsync

public override async Task OnConnectedAsync()
{
    await Clients.Others.SendAsync("UserIsOnline", Context.User.GetUsername());
}
public override async Task OnDisconnectedAsync(Exception exception)
{
    await Clients.Others.SendAsync("UserIsOffline", Context.User.GetUsername());
    await base.OnDisconnectedAsync(exception);
}

# Step 2: Add SignalR service:

services.AddSignalR();

# Step 3: Add Map

app.MapHub<PresenceHub>("hubs/presence");

# Step 4: Authenticating with SignalR

options.Events = new JwtBearerEvents
{
    OnMessageReceived = context =>
    {
        var accessToken = context.Request.Query["access_token"];
        var path = context.HttpContext.Request.Path;
        if (!string.IsNullOrEmpty(accessToken) && path.StartsWithSegments("/hubs"))
        {
            context.Token = accessToken;
        }

        return Task.CompletedTask;
    }
};

# Step 5: AllowCredentials

.AllowCredentials()
```

dotnet remove package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet remove package Microsoft.EntityFrameworkCore.Design
dotnet remove package Microsoft.EntityFrameworkCore.SqlServer
dotnet remove package Microsoft.EntityFrameworkCore.Tools
