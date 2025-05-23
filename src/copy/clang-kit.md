# Clang Kit

## `.clang-format`

```text
IndentWidth: 4
TabWidth: 4
UseTab: Never
```

## `.clang-tidy`

```text
Checks: >
  clang-analyzer-*,
  bugprone-*,
  readability-*

FormatStyle: file

CheckOptions:
  - { key: readability-identifier-naming.NamespaceCase,          value: lower_case }
  - { key: readability-identifier-naming.ClassCase,              value: CamelCase  }
  - { key: readability-identifier-naming.StructCase,             value: CamelCase  }
  - { key: readability-identifier-naming.TemplateParameterCase,  value: CamelCase  }
  - { key: readability-identifier-naming.FunctionCase,           value: lower_case  }
  - { key: readability-identifier-naming.VariableCase,           value: lower_case }
  - { key: readability-identifier-naming.PrivateMemberSuffix,    value: _          }
  - { key: readability-identifier-naming.ProtectedMemberSuffix,  value: _          }
  - { key: readability-identifier-naming.MacroDefinitionCase,    value: UPPER_CASE }
  - { key: readability-identifier-naming.EnumConstantCase,         value: CamelCase }
  - { key: readability-identifier-naming.EnumConstantPrefix,       value: _         }
  - { key: readability-identifier-naming.ConstexprVariableCase,    value: CamelCase }
  - { key: readability-identifier-naming.ConstexprVariablePrefix,  value: _         }
  - { key: readability-identifier-naming.GlobalConstantCase,       value: CamelCase }
  - { key: readability-identifier-naming.GlobalConstantPrefix,     value: _         }
  - { key: readability-identifier-naming.MemberConstantCase,       value: CamelCase }
  - { key: readability-identifier-naming.MemberConstantPrefix,     value: _         }
  - { key: readability-identifier-naming.StaticConstantCase,       value: CamelCase }
  - { key: readability-identifier-naming.StaticConstantPrefix,     value: _         }
```
