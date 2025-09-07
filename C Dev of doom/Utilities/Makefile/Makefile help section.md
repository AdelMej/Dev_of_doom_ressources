# Makefile Help Target

Add this to your `Makefile` to provide a handy overview of available commands:

```makefile
.PHONY: help all clean test doom

help:
	@echo "Available targets:"
	@echo "  all    - Build all binaries"
	@echo "  clean  - Remove build files"
	@echo "  test   - Run unit tests"
	@echo "  doom   - Unleash Segfault City (WARNING: may cause chaos!)"
```

Now you can run:

```bash
make help
```
to see a list of commands with descriptions.

It’s a great way to document your Makefile and help collaborators quickly understand what’s available.