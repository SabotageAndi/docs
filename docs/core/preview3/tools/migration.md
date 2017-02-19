---
title: .NET Core Application Deployment | Microsoft Docs
description: .NET Core project.json to csproj migration
keywords: .NET, .NET Core, .NET Core migration
author: blackdwarf
ms.author: mairaw
ms.date: 02/17/2017
ms.topic: article
ms.prod: .net-core
ms.devlang: dotnet
ms.assetid: 1feadf3d-3cfc-41dd-abb5-a4fc303a7b53
---

# Migrating .NET Core projects

[!INCLUDE[preview-warning](../../../includes/warning.md)]

This document will cover migration scenarios for .NET Core projects. It will cover the following three migration scenarios:

1. Migration from a valid latest schema of `project.json` to `csproj`
2. Migration from DNX to a valid Preview 2 project.json
3. Migration from RC3 and previous .NET Core csproj projects to the final format 

## Migration from project.json to csproj
Migration from project.json to csproj can be done via the [`dotnet migrate`](dotnet-migrate.md) command-line tool or through Visual Studio 2017. Both of these migrations use the same underlying engine to migrate projects, which means that the results will be the same regardless of the method you choose. 

Visual Studio 2017 will migrate the project automatically on opening either the `xproj` file or the solution file which references `xproj` files. 

Automatic migration translates concepts from project.json to semantically equivalvent concepts in csproj.  

The easiest option is to open up the solution in the 

## Migration from DNX to Preview 2 project.json

## Migration from earlier .NET Core csproj formats to RTM csproj
Prior to the final shape of .NET Core csproj format, there were variations that you could've started using. There is no tool that will migrate your project between these states, so the migration has to be done manually by editing the project file. The actual steps depend on the state of the project you are migrating. 

* Remove the tools version property from the `<Project>` element if it exists. 
* Remove the XML namespace (`xmlns`) from the `<Project>` element.
* If it doesn't exist, add the `Sdk` attribute to the `<Project>` element and set it to the value of `Microsoft.NET.Sdk`. This attribute specifies that the project uses the Core SDK. 
* Remove the `<Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" />` and `<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />` statements from the top and bottom of the project. These import statements are implied by the SDK, so there is no need for them to be in the project. 
* If you have `Microsoft.NETCore.App` or `NETStandard.Library` `<PackageReference>` items in your project, you should remove them. The SDK has these references implied. 
* Remove the `Microsoft.NET.Sdk` `<PackageReference>` element if it exists. The SDK reference comes through the `Sdk` attribute on the `<Project>` element. 
* Remove those globs that are [implied by the SDK](https://aka.ms/sdkimplititems). Leaving these globs in your project will cause an error on build because compile items will be duplicated. 

After these steps your project should be fully compatible with the v1 .NET Core csproj format. 