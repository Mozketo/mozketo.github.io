---
layout: post
title: Sql Compact and Fluent NHibernate
date: 2009-08-06 22:16
author: bclarkrobinson
comments: true
categories: [Fluent-NHibernate, Uncategorized]
---
I wanted a drop dead simple Fluent NHibernate connection to a Sql Compact (.sdf) file and was able to use:

<pre lang="CSharp" colla="+">
private const string DbFile = "firstProgram.db";

return Fluently.Configure()
    .Database(MsSqlCeConfiguration.Standard.ShowSql().ConnectionString(c =>
        c.Is("data source=" + dbFile))
        )
    .Mappings(m =>
        m.FluentMappings.AddFromAssemblyOf<Program>()
        )
    .ExposeConfiguration(BuildSchema)
    .BuildSessionFactory();
</pre>

Whereas the Sqlite connection was:

<pre lang="CSharp" colla="+">
private const string DbFile = "firstProgram.db";

 return Fluently.Configure()
    .Database(SQLiteConfiguration.Standard
        .UsingFile(dbFile)
        )
    .Mappings(m =>
        m.FluentMappings.AddFromAssemblyOf<Program>()
        )
    .ExposeConfiguration(BuildSchema)
    .BuildSessionFactory();
</pre>

When Googling I wasn't able to find any samples so I hope to fill that void.
