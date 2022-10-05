## Задача
В базе данных MS SQL Server есть продукты и категории. Одному продукту может соответствовать мнгоо категорий, в одной категории может быть много продуктов.
В конце находится SQL запрос для выбора всех пар «Имя продукта – Имя категории». Если у продукта нет категорий, то его имя все равно выводиться.


## Скрипт создания базы данных и данных к ней
```sql
/****** Object:  Table [dbo].[Category]    Script Date: 10/5/2022 7:45:56 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Category](
	[Id] [int] NOT NULL,
	[Name] [nvarchar](50) NULL,
 CONSTRAINT [PK_Category] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
/****** Object:  Table [dbo].[Product]    Script Date: 10/5/2022 7:45:56 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Product](
	[Id] [int] NOT NULL,
	[Name] [nvarchar](50) NULL,
 CONSTRAINT [PK_Product] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
/****** Object:  Table [dbo].[ProductCategory]    Script Date: 10/5/2022 7:45:56 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[ProductCategory](
	[Id] [int] NOT NULL,
	[IdProduct] [int] NOT NULL,
	[IdCategory] [int] NOT NULL,
 CONSTRAINT [PK_ProductCategory] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
INSERT [dbo].[Category] ([Id], [Name]) VALUES (1, N'Fruit')
GO
INSERT [dbo].[Category] ([Id], [Name]) VALUES (2, N'Cars')
GO
INSERT [dbo].[Category] ([Id], [Name]) VALUES (5, NULL)
GO
INSERT [dbo].[Category] ([Id], [Name]) VALUES (6, N'Water')
GO
INSERT [dbo].[Product] ([Id], [Name]) VALUES (1, N'Apple')
GO
INSERT [dbo].[Product] ([Id], [Name]) VALUES (2, N'Banan')
GO
INSERT [dbo].[Product] ([Id], [Name]) VALUES (3, N'Car')
GO
INSERT [dbo].[Product] ([Id], [Name]) VALUES (4, N'MooBike')
GO
INSERT [dbo].[Product] ([Id], [Name]) VALUES (5, N'Pepsi')
GO
INSERT [dbo].[Product] ([Id], [Name]) VALUES (6, N'Milk')
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (1, 1, 1)
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (2, 2, 1)
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (3, 3, 2)
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (4, 4, 2)
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (5, 5, 5)
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (6, 5, 6)
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (7, 6, 6)
GO
INSERT [dbo].[ProductCategory] ([Id], [IdProduct], [IdCategory]) VALUES (8, 6, 1)
GO
ALTER TABLE [dbo].[ProductCategory]  WITH CHECK ADD  CONSTRAINT [FK_ProductCategory_Category] FOREIGN KEY([IdCategory])
REFERENCES [dbo].[Category] ([Id])
GO
ALTER TABLE [dbo].[ProductCategory] CHECK CONSTRAINT [FK_ProductCategory_Category]
GO
ALTER TABLE [dbo].[ProductCategory]  WITH CHECK ADD  CONSTRAINT [FK_ProductCategory_Product] FOREIGN KEY([IdProduct])
REFERENCES [dbo].[Product] ([Id])
GO
ALTER TABLE [dbo].[ProductCategory] CHECK CONSTRAINT [FK_ProductCategory_Product]
GO
USE [master]
GO
ALTER DATABASE [ProductCategory] SET  READ_WRITE 
GO
```

## Выборка данных
```sql
SELECT Product.Name,Category.Name FROM dbo.Product
left join dbo.ProductCategory
on dbo.ProductCategory.IdProduct = dbo.Product.Id
left join dbo.Category
on dbo.Category.Id = dbo.ProductCategory.IdCategory
```


## Ожидаемый результат
```
Name	Name
Apple	Fruit
Banan	Fruit
Car	    Cars
MooBike	Cars
Pepsi	NULL
Pepsi	Water
Milk	Water
Milk	Fruit
```