# Makefile for AsociaBet API Documentation

.PHONY: setup install serve build clean

# Default target
all: install serve

# Setup virtual environment
setup:
	python -m venv venv

# Install dependencies
install:
	pip install mkdocs mkdocs-material pymdown-extensions

# Serve documentation locally
serve:
	mkdocs serve

# Build documentation
build:
	mkdocs build

# Clean build artifacts
clean:
	rm -rf site/

# Deploy documentation (if you have a deployment script)
deploy: build
	echo "Deploying documentation..."
	# Add your deployment commands here