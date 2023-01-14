# dating-app-csharp-angular

## ASPDOTNET

```bash
dotnet new list: Xem các tính năng của dotnet có thể làm được
dotnet new sln: Tạo một dự án mới theo tên thư mục
dotnet new webapi -n API: Tạo template webapi
dotnet sln add API: Thêm template API (solution) vừa tạo vào dự án
dotnet sln list: Liệt kê các solution có trong dự án vừa tạo
```

Để chạy một solution, phải cd vào trong thư mực đó sau đó chạy câu lệnh sau:

```bash

dotnet run

dotnet run -lp https: Chạy với https
dotnet dev-certs https —clean: Clear hết certificate cũ để tạo một certificate mới
dotnet dev-certs https —trust: Trust certificate vừa tạo

dotnet restore: Để cập nhật lại các package trong file csproj mỗi khi thêm/xoá package

dotnet watch run: Để run code mới nhất, không cần phải chạy lại server mỗi khi code thay đổi (Hot Reload)
dotnet tool list -g: Để xem các package đã được installed
dotnet ef migrations add InitialCreate -o Data/Migrations: Tạo script migration từ Entity
dotnet ef database update: Để thực thi các script vừa tạo vào trong database

dotnet ef migrations add UserPasswordAdded

```

Để drop database

```bash
dotnet ef database drop
```

Để update một database

```bash
dotnet ef database update
```

## Angular

ng g —help

- Add a component in angular

```bash
ng g c nav --skip-tests
```

- Add a service in angular

```bash
ng g s \_services/account --skip-tests
```

In order to pass props from Parent to child component

- In the child component, we define a @Input annotation like this

```bash
@Input() usersFromHomeComponent: any;
```

- And in the parent component, we will pass like this

```bash
<app-register [usersFromHomeComponent]="users"></app-register>
```

- Add guard to the angular

```bash
ng g g _guards/auth --skip-tests
```

- Add interceptor to the angular

```bash
ng g interceptor \_
```
