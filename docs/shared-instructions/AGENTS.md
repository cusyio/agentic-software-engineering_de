<!--
SPDX-FileCopyrightText: 2026 cusy GmbH

SPDX-License-Identifier: BSD-3-Clause
-->
# General procedure
- Before creating code, brainstorm 5 different approaches to solve the problem and sort them by their probable effectiveness. Then, choose the best approach and implement it.

# uv
- Use `uv` to manage Python environments and dependencies.
- Use `uv run` to execute Python scripts and commands.
- Don't edit `pyproject.toml` directly. Instead, use `uv add` and `uv add --dev` to manage dependencies.

# Code quality and linting
- Use ruff, ty, prek, wily for code quality and linting.
- Run appropriate tooling after making changes to your code to ensure it meets quality standards.

# Typing
- Don't use excessive casting. If you find yourself needing to cast types frequently, consider refactoring your code to use more appropriate types. Casting should only be done in boundary layers where you are interfacing with external systems.
- Use type hints for all function parameters and return types.

# Testing
- Use `pytest` for testing your code.
- Collect pytest fixtures in a `conftest.py` file to avoid duplication.
- Use the `hypothesis` library for property-based testing when you have complex input spaces or need to test edge cases.
- Favor pytest monkeypatch to mock.
- When a test fails, run the last failed test first using `uv run pytest --last-failed`.
- Use Test Driven Development (TDD) for all code you write. Write tests before writing the implementation code.
- When you come across a bug or regression, think hard about writing a test and also how to create code that will prevent this from a happening again in the future.
- Prefer testing real code where possible. Use mocks and `monkeypatch` when absolute necessary. Try to avoid mocking as much as possible.

# Documentation
- Use google-style docstrings for all functions and classes you create.
- Include doctests in the docstrings of your functions to provide examples

# Logging
- Use logging to provide insight into failures. Don’t use print for debugging. Don’t use logging to hide stack traces if you are going to fail anyway.
- Don't hide exceptions. Let them propagate up to the caller. If you need to catch an exception, log it and re-raise it.

# Command line interface
- When creating a command line interface, add `--verbose` flag that provides logging output useful for debugging issues.
