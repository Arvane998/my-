import ast
import sys
from typing import Dict, Any

class CodeAnalyzer(ast.NodeVisitor):
    def __init__(self):
        self.function_count = 0
        self.class_count = 0
        self.import_count = 
        self.assignments = 0

    def visit_FunctionDef(self, node):
        self.function_count += 1
        self.generic_visit(node)

    def visit_ClassDef(self, node):
        self.class_count += 1
        self.generic_visit(node)

    def visit_Import(self, node):
        self.import_count += 1
        self.generic_visit(node)

    def visit_ImportFrom(self, node):
        self.import_count += 1
        self.generic_visit(node)

    def visit_Assign(self, node):
        self.assignments += 1
        self.generic_visit(node)


def analyze_code(source_code: str) -> Dict[str, Any]:
    try:
        tree = ast.parse(source_code)
        analyzer = CodeAnalyzer()
        analyzer.visit(tree)
        results = {
            "lines": len(source_code.splitlines()),
            "functions": analyzer.function_count,
            "classes": analyzer.class_count,
            "imports": analyzer.import_count,
            "assignments": analyzer.assignments
        }
        return results
    except SyntaxError as e:
        return {"error": f"Invalid Python code: {e}"}


def main():
    if len(sys.argv) < 2:
        print("Usage: python analyzer.py <file.py>")
        sys.exit(1)

    filename = sys.argv[1]
    try:
        with open(filename, "r", encoding="utf-8") as f:
            source_code = f.read()
        results = analyze_code(source_code)
        print("\n--- Code Analysis Report ---")
        for key, value in results.items():
            print(f"{key.capitalize()}: {value}")
    except FileNotFoundError:
        print("File not found.")
    except Exception as e:
        print(f"Unexpected error: {e}")


if __name__ == "__main__":
    main()
