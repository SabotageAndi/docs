---
title: dotnet-build command | Microsoft Docs
description: The dotnet-build command builds a project and all of its dependencies. 
keywords: dotnet-build, CLI, CLI command, .NET Core
author: blackdwarf
ms.author: mairaw
ms.date: 10/13/2016
ms.topic: article
ms.prod: .net-core
ms.technology: dotnet-cli
ms.devlang: dotnet
ms.assetid: 5e1a2bc4-a919-4a86-8f33-a9b218b1fcb3
---

#dotnet-build (.NET Core Tools RC4)

> [!WARNING]
> This topic applies to .NET Core Tools RC4. For the .NET Core Tools Preview 2 version,
> see the [dotnet-build](../../tools/dotnet-build.md) topic.

## Name 
dotnet-build -- Builds a project and all of its dependencies 

## Synopsis

`dotnet build [--help] [--output]  [--framework]  
    [--configuration]  [--runtime] [--version-suffix]
    [--build-profile]  [--no-incremental] [--no-dependencies]
    [<project>]`

## Description

The `dotnet build` command builds the project and its dependencies into a set of binaries. The binaries are the symbol files used for debugging (having a `*.pdb` extension) as well as the project's code in Intermediate Language (IL) with an `*.dll` extension. Additionally, a JSON file that lists out the dependencies of the application with the `*.deps` extension will be produced. Finally, a `runtime.config.json` file will be produced as well. This file specifies which shared runtime and version the built code will run against. 

If the project has third-party dependencies, such as libraries from NuGet, these will be resolved from the NuGet cache and will not be available with the project's built output. With that in mind, the product of `dotnet build` is not ready to be transferred to another machine to run. This is in contrast to the behavior of .NET Framework in which building an executable project (an application) will produce an output that is possible to run on any machine that has .NET Framework installed. In order to get a similar experience in .NET Core, you have to use the [dotnet publish](dotnet-publish.md) command. More information about this can be found in the [.NET Core Application Deployment](../deploying/index.md) document. 

Building requires the existence of an asset file (a file that lists all of the dependencies of your application), which 
means that you have to run [`dotnet restore`](dotnet-restore.md) prior to building your code.

`dotnet build` uses MSBuild to build the project, thus it support both parallel builds and 

**TODO: add msbuild properties**

Whether the project is executable or not is determined by the `<OutputType>` property in the project file. The following example shows a project that will produce executable code: 


```xml
<PropertyGroup>
    <OutputType>Exe</OutputType>
</PropertyGroup>
```

In order to produce a library, simply omit that property. 

## Options

`-h|--help`

Prints out a short help for the command.  

`-o|--output <OUTPUT_DIRECTORY>`

Directory in which to place the built binaries. You also need to define `--framework` when you specify this option.

`-f|--framework <FRAMEWORK>`

Compiles for a specific framework. The framework needs to be defined in the [project file](csproj.md).

`-c|--configuration [Debug|Release]`

Defines a configuration under which to build.  If omitted, it defaults to `Debug`.

`-r|--runtime [RUNTIME_IDENTIFIER]`

Target runtime to build for. For a list of Runtime Identifiers (RIDs) you can use, see the [RID catalog](../../rid-catalog.md). 

`--version-suffix [VERSION_SUFFIX]`

Defines what `*` should be replaced with in the version field in the project file. The format follows NuGet's version guidelines. 

`--build-profile`

Prints out the incremental safety checks that users need to address in order for incremental compilation to be automatically turned on.

`--no-incremental`

Marks the build as unsafe for incremental build. This turns off incremental compilation and forces a clean rebuild of the project dependency graph.

`--no-dependencies`

Ignores project-to-project references and only builds the root project specified to build.

## Examples

Build a project and its dependencies:

`dotnet build`

Build a project and its dependencies using Release configuration:

`dotnet build --configuration Release`

Build a project and its dependencies for a specific runtime (in this example, Ubuntu 16.04):

`dotnet build --runtime ubuntu.16.04-x64`
