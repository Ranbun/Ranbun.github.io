---
title: cmake 文件操作
date: 2022-09-10 01:36:07
categories:
- CMake
tags:
- fileSystem
---

记录一下在日常的工作中用到的一些`cmake`的关于文件操作的命令(基本`Copy`,`move`,`remove`$\dots$)

<!--more-->

## `cmake` 文件操作

- 通常使用`FILE`命令完成相关的参数.

### `Copy` File

```cmake
FILE(<COPY|INSTALL> <files>... DESTINATION <dir>
     [FILE_PERMISSIONS <permissions>...]
     [DIRECTORY_PERMISSIONS <permissions>...]
     [NO_SOURCE_PERMISSIONS] [USE_SOURCE_PERMISSIONS]
     [FOLLOW_SYMLINK_CHAIN]
     [FILES_MATCHING]
     [[PATTERN <pattern> | REGEX <regex>]
    [EXCLUDE] [PERMISSIONS <permissions>...]] [...])

# The COPY signature copies files, directories, and symlinks to a destination folder. 
# Relative input paths are evaluated with respect to the current source directory, and a relative destination is evaluated with respect to the current build directory. 
# Copying preserves input file timestamps, and optimizes out a file if it exists at the destination with the same timestamp.
# Copying preserves input permissions unless explicit permissions or NO_SOURCE_PERMISSIONS are given (default is USE_SOURCE_PERMISSIONS).

# If FOLLOW_SYMLINK_CHAIN is specified, COPY will recursively resolve the symlinks at the paths given until a real file is found, and install a corresponding symlink in the destination for each symlink encountered.
# For each symlink that is installed, the resolution is stripped of the directory, leaving only the filename, meaning that the new symlink points to a file in the same directory as the symlink. 
# This feature is useful on some Unix systems, where libraries are installed as a chain of symlinks with version numbers, with less specific versions pointing to more specific versions. 
# FOLLOW_SYMLINK_CHAIN will install all of these symlinks and the library itself into the destination directory. 
# For example, if you have the following directory structure:

```

- `file`命令的`copy`操作会将文件，目录，或者是符号链接复制到目标目录，相对输入路径是相对于当前源目录(`cmake文件`)，复制会保留输入文件的时间戳，如果文件存在于目标位置且具有相同的时间戳，则会对其进行优化。复制行为默认保留默认权限，除非手动指定(`NO_SOURCE_PERMISSIONS`)。

#### example

```cmake
# 拷贝一个目录({CMAKE_SOURCE_DIR}/resources/Fonts)到"{CMAKE_SOURCE_DIR}/bin/resources/"路径下
FILE(COPY ${CMAKE_SOURCE_DIR}/resources/Fonts DESTINATION ${CMAKE_SOURCE_DIR}/bin/resources/)
```

- load$\dots$ $\dots$
