# C++ Preferences

The technology the user prefers for a project in C++. It is the starting point of the recommendation, weighed with what the research finds (`shared/technology.md`).

## Language

C++26.

## Build and Packages

- Build system: xmake.
- Packages: xrepo, the package manager of xmake. Each third-party package comes from the official xrepo repository; a package it does not have comes from a self-hosted xrepo repository.

## Libraries

- Concurrency and asynchronous I/O, for a product that needs them: stdexec and Asio as the core libraries underneath, bridged by `exec::asio::use_sender`, which turns an Asio asynchronous operation into a stdexec sender.
- Unit tests: GoogleTest.
- Logging: spdlog.
- JSON: nlohmann_json.
- Command-line arguments: argparse.

## Code Quality

- Fuzzing and the sanitizers are a first-class investment, to reduce the bugs and the vulnerabilities of the C++ code.
- Static analysis: clang-tidy.

## Code Style

- **Formatting**: `.clang-format` based on the LLVM preset, with an indent of 4 spaces, a line width of 120, `*` and `&` next to the type, and opening braces on the same line. The includes are grouped, and sorted within each group: the file's own header, then the project's headers, the third-party headers, and the standard library headers. Headers use `#pragma once`. `.editorconfig` covers the files that are not C++.
- **Naming**, in the style of the standard library: types, functions, variables, and concepts in snake_case. Private and protected member variables take the prefix `m_`; public member variables, the fields of aggregates and plain data structures, take no prefix or suffix. Template parameters are in PascalCase. File names are in snake_case, with the extensions `.cpp` and `.hpp`.
