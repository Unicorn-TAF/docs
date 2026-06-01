---
title: File Allocator
category: "Helpers & utilities"
weight: 7
order: 2
---

## Overview
`FileAllocator` is a utility class that provides functionality to wait for and allocate files in a file system. It is particularly useful for scenarios involving file downloads or file system monitoring.

Namespace: `Unicorn.Taf.Core.Utility`


## Behavior

 - The class uses a polling interval of 500ms by default for checking file existence
 - When the file name is unknown, it tracks newly appeared files in the directory
 - Multiple matching files will throw a `FileNotFoundException`
 - The class automatically handles both absolute and relative file paths


## Usage Examples

### Example 1: Allocating a File with Unknown Name
```csharp
string downloadFolder = @"C:\Downloads";
var allocator = new FileAllocator(downloadFolder);

// Optional: set expected name part for better filtering
allocator.ExpectedFileNamePart = "report";

// Optional: exclude temporary files
allocator.ExcludeFileNames(".tmp", ".crdownload");

TimeSpan timeout = TimeSpan.FromSeconds(30);
string allocatedFile = allocator.WaitForFileToAppear(timeout);

Console.WriteLine($"File allocated: {allocatedFile}");
```

### Example 2: Waiting for a Specific File
```csharp
string downloadFolder = @"C:\Downloads";
string expectedFile = "document.pdf";

var allocator = new FileAllocator(downloadFolder, expectedFile);

TimeSpan timeout = TimeSpan.FromSeconds(15);
string file = allocator.WaitForFileToAppear(timeout);

Console.WriteLine($"File found: {file}");
```
