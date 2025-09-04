# Claude Code Instructions for CSV Editor Development

## Project Context
CSV Editor is a Model Context Protocol (MCP) server that provides AI assistants with 40+ tools for CSV data manipulation. Built with FastMCP, Pandas, and modern Python tooling.

## Development Guidelines

### Package Management
- Use `uv` for all Python operations (not pip or poetry)
- Run tests with `uv run test` or `uv run pytest`
- Use `uv run python` instead of direct `python` commands
- Install dependencies with `uv add <package>` or `uv add --dev <package>`

### Code Quality Standards
- Run `uv run all-checks` before committing (includes lint, format, type-check, test)
- Use `uv run ruff check` for linting
- Use `uv run black .` for formatting  
- Use `uv run mypy src/` for type checking
- Maintain test coverage above 80%

### Testing Approach
- Write comprehensive tests for all new MCP tools
- Use pytest fixtures for session management
- Test both success and error cases
- Include integration tests for tool workflows
- Run `uv run test-cov` to check coverage

### Version Management
- Primary version source: `pyproject.toml`
- Sync versions with `uv run sync-versions` after version changes
- Code uses dynamic version loading via `importlib.metadata`

### MCP Tool Development
- Place new tools in appropriate `tools/` modules
- Follow existing patterns for error handling and response structure
- Include comprehensive docstrings with examples
- Use type hints for all parameters and return values
- Handle null values correctly (JSON null → Python None → pandas NaN)

### Environment Configuration
- All environment variables use `CSV_EDITOR_` prefix
- Configuration centralized in `CSVSettings` class in `csv_session.py`
- Default values defined in the Settings class, not scattered `os.getenv()` calls

### Architecture Notes
- Session-based design with automatic cleanup
- Auto-save functionality enabled by default
- Persistent history with undo/redo capabilities
- Type-safe operations using Pydantic validation
- Modular tool organization for maintainability

## Common Commands
```bash
# Setup and development
uv sync                 # Install all dependencies
uv run csv-editor      # Run the MCP server
uv run test            # Run test suite
uv run all-checks      # Full quality check pipeline

# Version management  
uv run sync-versions   # Sync version numbers across files

# Documentation
cd docs && npm run build  # Build Docusaurus site
cd docs && npm run serve  # Serve docs locally
```

## File Structure Context
- `src/csv_editor/` - Main package code
- `tests/` - Test suite mirroring source structure
- `examples/` - Usage examples and demos
- `docs/` - Docusaurus documentation site
- `scripts/` - Maintenance and utility scripts

## Key Implementation Details
- FastMCP framework for MCP protocol
- Pydantic models for data validation
- Pandas for data operations
- Session management with configurable timeouts
- Comprehensive error handling and logging