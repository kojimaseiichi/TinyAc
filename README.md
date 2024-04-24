# TinyAc
個人商店で利用する、売り上げ、原価管理システム。

## ソリューション・プロジェクト作成
```sh
# ソリューション作成
dotnet new sln

# UIプロジェクト作成
mkdir tinyac
cd tinyac
dotnet new blazor -int Server -au Individual --no-https
cd ..
dotnet sln add "tinyac/tinyac.csproj"

# DBプロジェクト作成
mkdir db
cd db
dotnet new classlib
dotnet add package Microsoft.EntityFrameworkCore.Sqlite --version 8.0.4
dotnet add package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore --version 8.0.4
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore --version 8.0.4
dotnet add package Microsoft.EntityFrameworkCore.Design --version 8.0.4
cd ..
dotnet sln add "db/db.csproj"

# 業務ロジック実装プロジェクト作成
mkdir biz
cd biz
dotnet new classlib
cd ..
dotnet sln add "biz/biz.csproj"


dotnet add biz/biz.csproj reference db/db.csproj
dotnet add tinyac/tinyac.csproj reference biz/biz.csproj
```

## SQLite移行を適用
```sh
# サブコマンドdotnet efに必要
# dotnet tool install --global dotnet-ef
dotnet ef migrations add CreateIdentitySchema --project db/db.csproj --startup-project tinyac/tinyac.csproj
dotnet ef database update --project db/db.csproj --startup-project tinyac/tinyac.csproj
```