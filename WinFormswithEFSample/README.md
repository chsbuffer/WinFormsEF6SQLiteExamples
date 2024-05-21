# Databinding with WinForms using EF6 System.Data.SQLite Code-First 

# Getting Started

Most of the code was copy-pasting from https://learn.microsoft.com/en-us/ef/ef6/fundamentals/databinding/winforms

# SQLite EF6

Home: https://system.data.sqlite.org/   
Package: https://www.nuget.org/packages/System.Data.SQLite

The `System.Data.SQLite.EF6` provider will automatically added to `entityFramework`->`providers` section when adding the nuget package. 
```diff
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
  </startup>
  <entityFramework>
    <providers>
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
+      <provider invariantName="System.Data.SQLite.EF6" type="System.Data.SQLite.EF6.SQLiteProviderServices, System.Data.SQLite.EF6" />
    </providers>
  </entityFramework>
+  <system.data>
+    <DbProviderFactories>
+      <remove invariant="System.Data.SQLite.EF6" />
+      <add name="SQLite Data Provider (Entity Framework 6)" invariant="System.Data.SQLite.EF6" description=".NET Framework Data Provider for SQLite (Entity Framework 6)" type="System.Data.SQLite.EF6.SQLiteProviderFactory, System.Data.SQLite.EF6" />
+    <remove invariant="System.Data.SQLite" /><add name="SQLite Data Provider" invariant="System.Data.SQLite" description=".NET Framework Data Provider for SQLite" type="System.Data.SQLite.SQLiteFactory, System.Data.SQLite" /></DbProviderFactories>
+  </system.data>
</configuration>
```   
Then I renamed the provider to let things just works.
```diff
...
  <entityFramework>
    <providers>
-      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
-      <provider invariantName="System.Data.SQLite.EF6" type="System.Data.SQLite.EF6.SQLiteProviderServices, System.Data.SQLite.EF6" />
+      <provider invariantName="System.Data.SQLite" type="System.Data.SQLite.EF6.SQLiteProviderServices, System.Data.SQLite.EF6" />
    </providers>
  </entityFramework>
...
```

## SQLite Code-First
EF6 SQLite Code-First support by https://github.com/msallin/SQLiteCodeFirst

``` csharp
        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            var sqliteConnectionInitializer = new SqliteCreateDatabaseIfNotExists<ProductContext>(modelBuilder, nullByteFileMeansNotExisting: true);
            Database.SetInitializer(sqliteConnectionInitializer);
        }
```

# What can goes wrong

- The project configuration must be **AnyCPU** for **Add New Data Source** -> **Object** to include your entity model classes.