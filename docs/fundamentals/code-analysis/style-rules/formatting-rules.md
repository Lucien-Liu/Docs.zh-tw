---
title: 程式碼樣式格式化規則
description: 深入瞭解格式化縮排、空格和新行的程式碼樣式規則。
ms.date: 09/25/2020
ms.topic: reference
author: gewarren
ms.author: gewarren
dev_langs:
- CSharp
- VB
f1_keywords:
- IDE0055
- formatting rules
helpviewer_keywords:
- IDE0055
- formatting code style rules [EditorConfig]
- formatting rules
- EditorConfig formatting conventions
ms.openlocfilehash: 61e6f6a6afdc6aaf9710eef3143af8ae700ef612
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2020
ms.locfileid: "96586280"
---
# <a name="formatting-rules"></a>格式化規則

格式化規則會影響縮排、空格和新行如何在 .NET 程式設計語言結構的周圍對齊。 規則分成下列類別：

- [.Net 格式化規則](#net-formatting-rules)：適用于 c # 和 Visual Basic 的規則。 這些規則的 EditorConfig 選項名稱會以前置詞開頭 `dotnet_` 。
- [C # 格式化規則](#c-formatting-rules)：僅適用于 c # 語言的規則。 這些規則的 EditorConfig 選項名稱會以前置詞開頭 `csharp_` 。

## <a name="rule-id-ide0055-fix-formatting"></a>規則識別碼： "IDE0055" (修正格式) 

所有格式化選項都有規則識別碼 `IDE0055` 和標題 `Fix formatting` 。 使用下列設定行，在 EditorConfig 檔中設定格式違規的嚴重性。

```ini
dotnet_diagnostic.IDE0055.severity = <severity value>
```

嚴重性值必須是 `warning` 或 `error` [在組建時強制執行](../overview.md#code-style-analysis)。 如需所有可能的嚴重性值，請參閱 [嚴重性層級](../configuration-options.md#severity-level)。

## <a name="option-format"></a>選項格式

您可以使用下列格式，在 EditorConfig 檔中指定格式化規則的選項：

`rule_name = value`

對於許多格式來說，您可以針對 `value` 指定 `true` (慣用此樣式) 或 `false` (不喜好此樣式)。 針對其他規則，您可以指定像是 `flush_left` 或 `before_and_after` 這樣的值，以描述套用規則的時間與位置。 您不用指定嚴重性。

## <a name="net-formatting-rules"></a>.NET 格式化規則

本節中的格式化規則適用于 c # 和 Visual Basic。

- [召集人 using](#organize-using-directives)
  - dotnet_sort_system_directives_first
  - dotnet_separate_import_directive_groups

### <a name="organize-using-directives"></a>組織 using 指示詞

這些格式化規則是關於 `using` 指示詞和 `Imports` 陳述式的排序和顯示。

Editorconfig 檔案範例︰

```ini
# .NET formatting rules
[*.{cs,vb}]
dotnet_sort_system_directives_first = true
dotnet_separate_import_directive_groups = true
```

#### <a name="dotnet_sort_system_directives_first"></a>dotnet\_sort\_system\_directives_first

|屬性|值|
|-|-|
| **選項名稱** | dotnet_sort_system_directives_first |
| **適用的語言** | C# 和 Visual Basic |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` -依字母順序排序指示詞 `System.*` `using` ，並將它們放在其他 using 指示詞之前。<br /><br />`false` -請勿將指示詞放置 `System.*` `using` 在其他指示詞之前 `using` 。 |

程式碼範例：

```csharp
// dotnet_sort_system_directives_first = true
using System.Collections.Generic;
using System.Threading.Tasks;
using Octokit;

// dotnet_sort_system_directives_first = false
using System.Collections.Generic;
using Octokit;
using System.Threading.Tasks;
```

#### <a name="dotnet_separate_import_directive_groups"></a>dotnet\_separate\_import\_directive\_groups

|屬性|值|
|-|-|
| **選項名稱** | dotnet_separate_import_directive_groups |
| **適用的語言** | C# 和 Visual Basic |
| **引進的版本** | Visual Studio 2017 15.5 版 |
| **選項值** | `true` - 在 `using` 指示詞群組之間放置空白行。<br /><br />`false` - 在 `using` 指示詞群組之間放置空白行。 |

程式碼範例：

```csharp
// dotnet_separate_import_directive_groups = true
using System.Collections.Generic;
using System.Threading.Tasks;

using Octokit;

// dotnet_separate_import_directive_groups = false
using System.Collections.Generic;
using System.Threading.Tasks;
using Octokit;
```

## <a name="c-formatting-rules"></a>C # 格式化規則

本節中的格式化規則只適用於 C# 程式碼。

- [換行選項](#new-line-options)
  - csharp_new_line_before_open_brace
  - csharp_new_line_before_else
  - csharp_new_line_before_catch
  - csharp_new_line_before_finally
  - csharp_new_line_before_members_in_object_initializers
  - csharp_new_line_before_members_in_anonymous_types
  - csharp_new_line_between_query_expression_clauses
- [縮排選項](#indentation-options)
  - csharp_indent_case_contents
  - csharp_indent_switch_labels
  - csharp_indent_labels
  - csharp_indent_block_contents
  - csharp_indent_braces
  - csharp_indent_case_contents_when_block
- [間距選項](#spacing-options)
  - csharp_space_after_cast
  - csharp_space_after_keywords_in_control_flow_statements
  - csharp_space_between_parentheses
  - csharp_space_before_colon_in_inheritance_clause
  - csharp_space_after_colon_in_inheritance_clause
  - csharp_space_around_binary_operators
  - csharp_space_between_method_declaration_parameter_list_parentheses
  - csharp_space_between_method_declaration_empty_parameter_list_parentheses
  - csharp_space_between_method_declaration_name_and_open_parenthesis
  - csharp_space_between_method_call_parameter_list_parentheses
  - csharp_space_between_method_call_empty_parameter_list_parentheses
  - csharp_space_between_method_call_name_and_opening_parenthesis
  - csharp_space_after_comma
  - csharp_space_before_comma
  - csharp_space_after_dot
  - csharp_space_before_dot
  - csharp_space_after_semicolon_in_for_statement
  - csharp_space_before_semicolon_in_for_statement
  - csharp_space_around_declaration_statements
  - csharp_space_before_open_square_brackets
  - csharp_space_between_empty_square_brackets
  - csharp_space_between_square_brackets
- [包裝選項](#wrap-options)
  - csharp_preserve_single_line_statements
  - csharp_preserve_single_line_blocks
- [Using 指示詞選項](#using-directive-options)
  - csharp_using_directive_placement

### <a name="new-line-options"></a>新行選項

這些格式化規則是關於格式化程式碼新行的使用。

Editorconfig 檔案範例︰

```ini
# CSharp formatting rules:
[*.cs]
csharp_new_line_before_open_brace = methods, properties, control_blocks, types
csharp_new_line_before_else = true
csharp_new_line_before_catch = true
csharp_new_line_before_finally = true
csharp_new_line_before_members_in_object_initializers = true
csharp_new_line_before_members_in_anonymous_types = true
csharp_new_line_between_query_expression_clauses = true
```

#### <a name="csharp_new_line_before_open_brace"></a>csharp\_new\_line\_before\_open_brace

此規則是有關左大括弧 `{` 應該和前面的程式碼放在同一行還是放在新行中。 針對此規則，您可以指定 [全部]、[無] 或一或多個程式碼項目，例如 **方法** 或 **屬性**，來定義應於何時套用此規則。 若要指定多個程式碼項目，請使用逗號 (,) 區隔。

|屬性|值|
|-|-|
| **選項名稱** | csharp_new_line_before_open_brace |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `all` - 針對所有運算式要求大括弧位於新行上 ("Allman" 樣式)。<br /><br />`none` - 針對所有運算式要求大括弧位於同一行上 ("K&R")。<br /><br />`accessors`、`anonymous_methods`、`anonymous_types`、`control_blocks`、`events`、`indexers`、`lambdas`、`local_functions`、`methods`、`object_collection_array_initializers`、`properties`、`types` - 針對指定的程式碼項目要求大括弧位於新行 ("Allman" 樣式)。 |

程式碼範例：

```csharp
// csharp_new_line_before_open_brace = all
void MyMethod()
{
    if (...)
    {
        ...
    }
}

// csharp_new_line_before_open_brace = none
void MyMethod() {
    if (...) {
        ...
    }
}
```

#### <a name="csharp_new_line_before_else"></a>csharp\_new\_line\_before_else

|屬性|值|
|-|-|
| **選項名稱** | csharp_new_line_before_else |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 將 `else` 陳述式置於新行上。<br /><br />`false` - 將 `else` 陳述式置於同一行上。 |

程式碼範例：

```csharp
// csharp_new_line_before_else = true
if (...) {
    ...
}
else {
    ...
}

// csharp_new_line_before_else = false
if (...) {
    ...
} else {
    ...
}
```

#### <a name="csharp_new_line_before_catch"></a>csharp\_new\_line\_before_catch

|屬性|值|
|-|-|
| **選項名稱** | csharp_new_line_before_catch |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 將 `catch` 陳述式置於新行上。<br /><br />`false` - 將 `catch` 陳述式置於同一行上。 |

程式碼範例：

```csharp
// csharp_new_line_before_catch = true
try {
    ...
}
catch (Exception e) {
    ...
}

// csharp_new_line_before_catch = false
try {
    ...
} catch (Exception e) {
    ...
}
```

#### <a name="csharp_new_line_before_finally"></a>csharp\_new\_line\_before_finally

|屬性|值|
|-|-|
| **選項名稱** | csharp_new_line_before_finally |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 要求 `finally` 陳述式位於右大括號之後的新行上。<br /><br />`false` - 要求 `finally` 陳述式位於右大括號的同一行上。 |

程式碼範例：

```csharp
// csharp_new_line_before_finally = true
try {
    ...
}
catch (Exception e) {
    ...
}
finally {
    ...
}

// csharp_new_line_before_finally = false
try {
    ...
} catch (Exception e) {
    ...
} finally {
    ...
}
```

#### <a name="csharp_new_line_before_members_in_object_initializers"></a>csharp\_new\_line\_before\_members\_in\_object_initializers

|屬性|值|
|-|-|
| **選項名稱** | csharp_new_line_before_members_in_object_initializers |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 要求物件初始設定式的成員位於不同行上<br /><br />`false` - 要求物件初始設定式的成員位於同一行上 |

程式碼範例：

```csharp
// csharp_new_line_before_members_in_object_initializers = true
var z = new B()
{
    A = 3,
    B = 4
}

// csharp_new_line_before_members_in_object_initializers = false
var z = new B()
{
    A = 3, B = 4
}
```

#### <a name="csharp_new_line_before_members_in_anonymous_types"></a>csharp\_new\_line\_before\_members\_in\_anonymous_types

|屬性|值|
|-|-|
| **選項名稱** | csharp_new_line_before_members_in_anonymous_types |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 要求匿名類型的成員位於不同行上<br /><br />`false` - 要求匿名類型的成員位於同一行上 |

程式碼範例：

```csharp
// csharp_new_line_before_members_in_anonymous_types = true
var z = new
{
    A = 3,
    B = 4
}

// csharp_new_line_before_members_in_anonymous_types = false
var z = new
{
    A = 3, B = 4
}
```

#### <a name="csharp_new_line_between_query_expression_clauses"></a>csharp_new_line_between_query_expression_clauses

|屬性|值|
|-|-|
| **選項名稱** | csharp_new_line_between_query_expression_clauses |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 要求查詢運算式子句的元素位於不同行上<br /><br />`false` - 要求查詢運算式子句的元素位於同一行上 |

程式碼範例：

```csharp
// csharp_new_line_between_query_expression_clauses = true
var q = from a in e
        from b in e
        select a * b;

// csharp_new_line_between_query_expression_clauses = false
var q = from a in e from b in e
        select a * b;
```

### <a name="indentation-options"></a>縮排選項

這些格式化規則是關於格式化程式碼縮排的使用。

Editorconfig 檔案範例︰

```ini
# CSharp formatting rules:
[*.cs]
csharp_indent_case_contents = true
csharp_indent_switch_labels = true
csharp_indent_labels = flush_left
csharp_indent_block_contents = true
csharp_indent_braces = false
csharp_indent_case_contents_when_block = true
```

#### <a name="csharp_indent_case_contents"></a>csharp\_indent\_case_contents

|屬性|值|
|-|-|
| **選項名稱** | csharp_indent_case_contents |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 對 `switch` 案例內容進行縮排<br /><br />`false` - 不對 `switch` 案例內容進行縮排 |

- 當此規則設定為 **true**，為 i。
- 當此規則設為 **false** 時，為 d。

程式碼範例：

```csharp
// csharp_indent_case_contents = true
switch(c) {
    case Color.Red:
        Console.WriteLine("The color is red");
        break;
    case Color.Blue:
        Console.WriteLine("The color is blue");
        break;
    default:
        Console.WriteLine("The color is unknown.");
        break;
}

// csharp_indent_case_contents = false
switch(c) {
    case Color.Red:
    Console.WriteLine("The color is red");
    break;
    case Color.Blue:
    Console.WriteLine("The color is blue");
    break;
    default:
    Console.WriteLine("The color is unknown.");
    break;
}
```

#### <a name="csharp_indent_switch_labels"></a>csharp\_indent\_switch_labels

|屬性|值|
|-|-|
| **選項名稱** | csharp_indent_switch_labels |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 縮排 `switch` 標籤<br /><br />`false` - 不要縮排 `switch` 標籤 |

程式碼範例：

```csharp
// csharp_indent_switch_labels = true
switch(c) {
    case Color.Red:
        Console.WriteLine("The color is red");
        break;
    case Color.Blue:
        Console.WriteLine("The color is blue");
        break;
    default:
        Console.WriteLine("The color is unknown.");
        break;
}

// csharp_indent_switch_labels = false
switch(c) {
case Color.Red:
    Console.WriteLine("The color is red");
    break;
case Color.Blue:
    Console.WriteLine("The color is blue");
    break;
default:
    Console.WriteLine("The color is unknown.");
    break;
}
```

#### <a name="csharp_indent_labels"></a>csharp\_indent_labels

|屬性|值|
|-|-|
| **選項名稱** | csharp_indent_labels |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `flush_left` - 標籤放在最左邊的資料行<br /><br />`one_less_than_current` - 將標籤置於比目前內容的縮排少一個單位的位置<br /><br />`no_change` - 將標籤置於和目前內容相同縮排的位置 |

程式碼範例：

```csharp
// csharp_indent_labels= flush_left
class C
{
    private string MyMethod(...)
    {
        if (...) {
            goto error;
        }
error:
        throw new Exception(...);
    }
}

// csharp_indent_labels = one_less_than_current
class C
{
    private string MyMethod(...)
    {
        if (...) {
            goto error;
        }
    error:
        throw new Exception(...);
    }
}

// csharp_indent_labels= no_change
class C
{
    private string MyMethod(...)
    {
        if (...) {
            goto error;
        }
        error:
        throw new Exception(...);
    }
}
```

#### <a name="csharp_indent_block_contents"></a>csharp_indent_block_contents

|屬性|值|
|-|-|
| **選項名稱** | csharp_indent_block_contents |
| **適用的語言** | C# |
| **選項值** | `true` - <br /><br />`false` -  |

程式碼範例：

```csharp
// csharp_indent_block_contents = true
static void Hello()
{
    Console.WriteLine("Hello");
}

// csharp_indent_block_contents = false
static void Hello()
{
Console.WriteLine("Hello");
}
```

#### <a name="csharp_indent_braces"></a>csharp_indent_braces

|屬性|值|
|-|-|
| **選項名稱** | csharp_indent_braces |
| **適用的語言** | C# |
| **選項值** | `true` - <br /><br />`false` -  |

程式碼範例：

```csharp
// csharp_indent_braces = true
static void Hello()
    {
    Console.WriteLine("Hello");
    }

// csharp_indent_braces = false
static void Hello()
{
    Console.WriteLine("Hello");
}
```

#### <a name="csharp_indent_case_contents_when_block"></a>csharp_indent_case_contents_when_block

|屬性|值|
|-|-|
| **選項名稱** | csharp_indent_case_contents_when_block |
| **適用的語言** | C# |
| **選項值** | `true` - <br /><br />`false` -  |

程式碼範例：

```csharp
// csharp_indent_case_contents_when_block = true
case 0:
    {
        Console.WriteLine("Hello");
        break;
    }

// csharp_indent_case_contents_when_block = false
case 0:
{
    Console.WriteLine("Hello");
    break;
}
```

### <a name="spacing-options"></a>間距選項

這些格式化規則是關於格式化程式碼空白字元的使用。

Editorconfig 檔案範例︰

```ini
# CSharp formatting rules:
[*.cs]
csharp_space_after_cast = true
csharp_space_after_keywords_in_control_flow_statements = true
csharp_space_between_parentheses = control_flow_statements, type_casts
csharp_space_before_colon_in_inheritance_clause = true
csharp_space_after_colon_in_inheritance_clause = true
csharp_space_around_binary_operators = before_and_after
csharp_space_between_method_declaration_parameter_list_parentheses = true
csharp_space_between_method_declaration_empty_parameter_list_parentheses = false
csharp_space_between_method_declaration_name_and_open_parenthesis = false
csharp_space_between_method_call_parameter_list_parentheses = true
csharp_space_between_method_call_empty_parameter_list_parentheses = false
csharp_space_between_method_call_name_and_opening_parenthesis = false
csharp_space_after_comma = true
csharp_space_before_comma = false
csharp_space_after_dot = false
csharp_space_before_dot = false
csharp_space_after_semicolon_in_for_statement = true
csharp_space_before_semicolon_in_for_statement = false
csharp_space_around_declaration_statements = false
csharp_space_before_open_square_brackets = false
csharp_space_between_empty_square_brackets = false
csharp_space_between_square_brackets = false
```

#### <a name="csharp_space_after_cast"></a>csharp\_space\_after_cast

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_after_cast |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 在轉換和值之間放入一個空白字元<br /><br />`false` - 移除轉換和值之間的空格 |

程式碼範例：

```csharp
// csharp_space_after_cast = true
int y = (int) x;

// csharp_space_after_cast = false
int y = (int)x;
```

#### <a name="csharp_space_after_keywords_in_control_flow_statements"></a>csharp_space_after_keywords_in_control_flow_statements

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_after_keywords_in_control_flow_statements |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 在控制流程陳述式中的關鍵字後面放入一個空白字元，例如 `for` 迴圈<br /><br />`false` - 移除控制流程陳述式中關鍵字後面的空格，例如 `for` 迴圈 |

程式碼範例：

```csharp
// csharp_space_after_keywords_in_control_flow_statements = true
for (int i;i<x;i++) { ... }

// csharp_space_after_keywords_in_control_flow_statements = false
for(int i;i<x;i++) { ... }
```

#### <a name="csharp_space_between_parentheses"></a>csharp_space_between_parentheses

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_parentheses |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `control_flow_statements` - 在控制流程陳述式的括號之間加入空格<br /><br />`expressions` - 在運算式的括號之間加入空格<br /><br />`type_casts` - 在類型轉換中的括號之間加入空格 |

如果您略過此規則，或使用 `control_flow_statements`、`expressions` 或 `type_casts` 以外的值，即不套用設定。

程式碼範例：

```csharp
// csharp_space_between_parentheses = control_flow_statements
for ( int i = 0; i < 10; i++ ) { }

// csharp_space_between_parentheses = expressions
var z = ( x * y ) - ( ( y - x ) * 3 );

// csharp_space_between_parentheses = type_casts
int y = ( int )x;
```

#### <a name="csharp_space_before_colon_in_inheritance_clause"></a>csharp\_space\_before\_colon\_in\_inheritance_clause

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_before_colon_in_inheritance_clause |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.7 版 |
| **選項值** | `true` - 在型別宣告中基底或介面的冒號之前放入一個空白字元<br /><br />`false` - 移除型別宣告中基底或介面內冒號之前的空格 |

程式碼範例：

```csharp
// csharp_space_before_colon_in_inheritance_clause = true
interface I
{

}

class C : I
{

}

// csharp_space_before_colon_in_inheritance_clause = false
interface I
{

}

class C: I
{

}
```

#### <a name="csharp_space_after_colon_in_inheritance_clause"></a>csharp\_space\_after\_colon\_in\_inheritance_clause

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_after_colon_in_inheritance_clause |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.7 版 |
| **選項值** | `true` - 在型別宣告中基底或介面的冒號之後放入一個空白字元<br /><br />`false` - 移除型別宣告中基底或介面內冒號之後的空格 |

程式碼範例：

```csharp
// csharp_space_after_colon_in_inheritance_clause = true
interface I
{

}

class C : I
{

}

// csharp_space_after_colon_in_inheritance_clause = false
interface I
{

}

class C :I
{

}
```

#### <a name="csharp_space_around_binary_operators"></a>csharp\_space\_around\_binary_operators

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_around_binary_operators |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.7 版 |
| **選項值** | `before_and_after` - 在二元運算子前後插入空格<br /><br />`none` - 移除二元運算子前後的空格<br /><br />`ignore` - 忽略二元運算子前後的空格 |

如果您略過這項規則，或使用 `before_and_after`、`none` 或 `ignore` 以外的值，即不套用設定。

程式碼範例：

```csharp
// csharp_space_around_binary_operators = before_and_after
return x * (x - y);

// csharp_space_around_binary_operators = none
return x*(x-y);

// csharp_space_around_binary_operators = ignore
return x  *  (x-y);
```

#### <a name="csharp_space_between_method_declaration_parameter_list_parentheses"></a>csharp_space_between_method_declaration_parameter_list_parentheses

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_method_declaration_parameter_list_parentheses |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 在方法宣告參數清單的左括弧後面和右括弧前面加上空格字元<br /><br />`false` - 移除方法宣告參數清單的左括弧後面和右括弧前面的空白字元 |

程式碼範例：

```csharp
// csharp_space_between_method_declaration_parameter_list_parentheses = true
void Bark( int x ) { ... }

// csharp_space_between_method_declaration_parameter_list_parentheses = false
void Bark(int x) { ... }
```

#### <a name="csharp_space_between_method_declaration_empty_parameter_list_parentheses"></a>csharp_space_between_method_declaration_empty_parameter_list_parentheses

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_method_declaration_empty_parameter_list_parentheses |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.7 版 |
| **選項值** | `true` - 在方法宣告的空白參數清單括弧內插入空格<br /><br />`false` - 將方法宣告的空白參數清單括弧內的空格移除 |

程式碼範例：

```csharp
// csharp_space_between_method_declaration_empty_parameter_list_parentheses = true
void Goo( )
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}

// csharp_space_between_method_declaration_empty_parameter_list_parentheses = false
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}
```

#### <a name="csharp_space_between_method_declaration_name_and_open_parenthesis"></a>csharp_space_between_method_declaration_name_and_open_parenthesis

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_method_declaration_name_and_open_parenthesis |
| **適用的語言** | C# |
| **選項值** | `true` - 在方法名稱與方法宣告的左括弧之間放入一個空白字元<br /><br />`false` - 移除方法名稱與方法宣告左括弧之間的空白字元 |

程式碼範例：

```csharp
// csharp_space_between_method_declaration_name_and_open_parenthesis = true
void M () { }

// csharp_space_between_method_declaration_name_and_open_parenthesis = false
void M() { }
```

#### <a name="csharp_space_between_method_call_parameter_list_parentheses"></a>csharp_space_between_method_call_parameter_list_parentheses

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_method_call_parameter_list_parentheses |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 在方法呼叫的左括弧後面和右括弧前面加上空格字元<br /><br />`false` - 移除方法呼叫的左括弧後面和右括弧前面的空白字元 |

程式碼範例：

```csharp
// csharp_space_between_method_call_parameter_list_parentheses = true
MyMethod( argument );

// csharp_space_between_method_call_parameter_list_parentheses = false
MyMethod(argument);
```

#### <a name="csharp_space_between_method_call_empty_parameter_list_parentheses"></a>csharp_space_between_method_call_empty_parameter_list_parentheses

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_method_call_empty_parameter_list_parentheses |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.7 版 |
| **選項值** | `true` - 在空白引數清單括弧內插入空格<br /><br />`false` - 將空白引數清單括弧內的空格移除 |

程式碼範例：

```csharp
// csharp_space_between_method_call_empty_parameter_list_parentheses = true
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo( );
}

// csharp_space_between_method_call_empty_parameter_list_parentheses = false
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}
```

#### <a name="csharp_space_between_method_call_name_and_opening_parenthesis"></a>csharp_space_between_method_call_name_and_opening_parenthesis

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_method_call_name_and_opening_parenthesis |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.7 版 |
| **選項值** | `true` - 在方法呼叫名稱與左括弧之間插入空格<br /><br />`false` - 將方法呼叫名稱與左括弧之間的空格移除 |

程式碼範例：

```csharp
// csharp_space_between_method_call_name_and_opening_parenthesis = true
void Goo()
{
    Goo (1);
}

void Goo(int x)
{
    Goo ();
}

// csharp_space_between_method_call_name_and_opening_parenthesis = false
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}
```

#### <a name="csharp_space_after_comma"></a>csharp_space_after_comma

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_after_comma |
| **適用的語言** | C# |
| **選項值** | `true` - 在逗號後面插入空格<br /><br />`false` - 將逗號後面的空格移除 |

程式碼範例：

```csharp
// csharp_space_after_comma = true
int[] x = new int[] { 1, 2, 3, 4, 5 };

// csharp_space_after_comma = false
int[] x = new int[] { 1,2,3,4,5 }
```

#### <a name="csharp_space_before_comma"></a>csharp_space_before_comma

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_before_comma |
| **適用的語言** | C# |
| **選項值** | `true` - 在逗號之前插入空格<br /><br />`false` - 移除逗號之前的空格 |

程式碼範例：

```csharp
// csharp_space_before_comma = true
int[] x = new int[] { 1 , 2 , 3 , 4 , 5 };

// csharp_space_before_comma = false
int[] x = new int[] { 1, 2, 3, 4, 5 };
```

#### <a name="csharp_space_after_dot"></a>csharp_space_after_dot

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_after_dot |
| **適用的語言** | C# |
| **選項值** | `true` - 在點後面插入空格<br /><br />`false` - 將點後面的空格移除 |

程式碼範例：

```csharp
// csharp_space_after_dot = true
this. Goo();

// csharp_space_after_dot = false
this.Goo();
```

#### <a name="csharp_space_before_dot"></a>csharp_space_before_dot

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_before_dot |
| **適用的語言** | C# |
| **選項值** | `true` - 在點之前插入空格 <br /><br />`false` - 移除點之前的空格 |

程式碼範例：

```csharp
// csharp_space_before_dot = true
this .Goo();

// csharp_space_before_dot = false
this.Goo();
```

#### <a name="csharp_space_after_semicolon_in_for_statement"></a>csharp_space_after_semicolon_in_for_statement

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_after_semicolon_in_for_statement |
| **適用的語言** | C# |
| **選項值** | `true` - 在 `for` 陳述式的每個分號之後插入空格<br /><br />`false` - 移除 `for` 陳述式每個分號之後的空格 |

程式碼範例：

```csharp
// csharp_space_after_semicolon_in_for_statement = true
for (int i = 0; i < x.Length; i++)

// csharp_space_after_semicolon_in_for_statement = false
for (int i = 0;i < x.Length;i++)
```

##### <a name="csharp_space_before_semicolon_in_for_statement"></a>csharp_space_before_semicolon_in_for_statement

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_before_semicolon_in_for_statement |
| **適用的語言** | C# |
| **選項值** | `true` - 在 `for` 陳述式的每個分號之前插入空格 <br /><br />`false` - 移除 `for` 陳述式每個分號之前的空格 |

程式碼範例：

```csharp
// csharp_space_before_semicolon_in_for_statement = true
for (int i = 0 ; i < x.Length ; i++)

// csharp_space_before_semicolon_in_for_statement = false
for (int i = 0; i < x.Length; i++)
```

#### <a name="csharp_space_around_declaration_statements"></a>csharp_space_around_declaration_statements

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_around_declaration_statements |
| **適用的語言** | C# |
| **選項值** | `ignore` - 不要移除宣告陳述式中的額外空白字元<br /><br />`false` - 移除宣告陳述式中的額外空白字元 |

程式碼範例：

```csharp
// csharp_space_around_declaration_statements = ignore
int    x    =    0   ;

// csharp_space_around_declaration_statements = false
int x = 0;
```

#### <a name="csharp_space_before_open_square_brackets"></a>csharp_space_before_open_square_brackets

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_before_open_square_brackets |
| **適用的語言** | C# |
| **選項值** | `true` - 在左方括弧 `[` 之前插入空格 <br /><br />`false` - 移除左方括弧 `[` 之前的空格 |

程式碼範例：

```csharp
// csharp_space_before_open_square_brackets = true
int [] numbers = new int [] { 1, 2, 3, 4, 5 };

// csharp_space_before_open_square_brackets = false
int[] numbers = new int[] { 1, 2, 3, 4, 5 };
```

#### <a name="csharp_space_between_empty_square_brackets"></a>csharp_space_between_empty_square_brackets

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_empty_square_brackets |
| **適用的語言** | C# |
| **選項值** | `true` - 在空白方括弧 `[ ]` 之間插入空格 <br /><br />`false` - 移除空白方括弧 `[]` 之間的空格 |

程式碼範例：

```csharp
// csharp_space_between_empty_square_brackets = true
int[ ] numbers = new int[ ] { 1, 2, 3, 4, 5 };

// csharp_space_between_empty_square_brackets = false
int[] numbers = new int[] { 1, 2, 3, 4, 5 };
```

#### <a name="csharp_space_between_square_brackets"></a>csharp_space_between_square_brackets

|屬性|值|
|-|-|
| **選項名稱** | csharp_space_between_square_brackets |
| **適用的語言** | C# |
| **選項值** | `true` - 在非空白方括弧 `[ 0 ]` 中插入空白字元 <br /><br />`false` - 移除非空白方括弧 `[0]` 之間的空白字元 |

程式碼範例：

```csharp
// csharp_space_between_square_brackets = true
int index = numbers[ 0 ];

// csharp_space_between_square_brackets = false
int index = numbers[0];
```

### <a name="wrap-options"></a>包裝選項

這些格式化規則是有關陳述式和程式碼區塊的單一行與個別行的使用。

Editorconfig 檔案範例︰

```ini
# CSharp formatting rules:
[*.cs]
csharp_preserve_single_line_statements = true
csharp_preserve_single_line_blocks = true
```

#### <a name="csharp_preserve_single_line_statements"></a>csharp_preserve_single_line_statements

|屬性|值|
|-|-|
| **選項名稱** | csharp_preserve_single_line_statements |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 將陳述式和成員宣告保留在同一行上<br /><br />`false` - 將陳述式和成員宣告保留在不同的行上 |

程式碼範例：

```csharp
//csharp_preserve_single_line_statements = true
int i = 0; string name = "John";

//csharp_preserve_single_line_statements = false
int i = 0;
string name = "John";
```

#### <a name="csharp_preserve_single_line_blocks"></a>csharp_preserve_single_line_blocks

|屬性|值|
|-|-|
| **選項名稱** | csharp_preserve_single_line_blocks |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2017 15.3 版 |
| **選項值** | `true` - 將程式碼區塊保留在單行上<br /><br />`false` - 將程式碼區塊保留在不同的行上 |

程式碼範例：

```csharp
//csharp_preserve_single_line_blocks = true
public int Foo { get; set; }

//csharp_preserve_single_line_blocks = false
public int MyProperty
{
    get; set;
}
```

### <a name="using-directive-options"></a>Using 指示詞選項

此格式設定規則會考慮使用在命名空間內部與外部的 using 指示詞。

Editorconfig 檔案範例︰

```ini
# 'using' directive preferences
[*.cs]
csharp_using_directive_placement = outside_namespace
csharp_using_directive_placement = inside_namespace
```

#### <a name="csharp_using_directive_placement"></a>csharp_using_directive_placement

|屬性|值|
|-|-|
| **選項名稱** | csharp_using_directive_placement |
| **適用的語言** | C# |
| **引進的版本** | Visual Studio 2019 16.1 版 |
| **選項值** | `outside_namespace` -離開命名空間以外的 using 指示詞<br /><br />`inside_namespace` -保留命名空間內的 using 指示詞 |

程式碼範例：

```csharp
// csharp_using_directive_placement = outside_namespace
using System;

namespace Conventions
{

}

// csharp_using_directive_placement = inside_namespace
namespace Conventions
{
    using System;
}
```

## <a name="see-also"></a>另請參閱

- [語言規則](language-rules.md)
- [命名規則](naming-rules.md)
- [.NET 程式碼樣式規則參考](index.md)
