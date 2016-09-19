# Python #

Homebrew's python is newer and comes with pip, so

```
brew install python
```

## Structs

**Byte Order, Size, and Alignment**

| Character | Byte order             | Size     | Alignment |
| --------- | ---------------------- | -------- | --------- |
| `@`       | native                 | native   | native    |
| `=`       | native                 | standard | none      |
| `<`       | little-endian          | standard | none      |
| `>`       | big-endian             | standard | none      |
| `!`       | network (= big-endian) | standard | none      |

**Formats**

The 'Standard size' column refers to the size of the packed value in bytes when using standard size; that is, when the format string starts with one of `'<'`, `'>'`, `'!'` or `'='`. When using native size, the size of the packed value is platform-dependent.

| Format | C Type               | Python type        | Standard size | Notes    |
| ------ | -------------------- | ------------------ | ------------- | -------- |
| `x`    | pad byte             | no value           |               |          |
| `c`    | `char`               | string of length 1 | 1             |          |
| `b`    | `signed char`        | integer            | 1             | (3)      |
| `B`    | `unsigned char`      | integer            | 1             | (3)      |
| `?`    | `_Bool`              | bool               | 1             | (1)      |
| `h`    | `short`              | integer            | 2             | (3)      |
| `H`    | `unsigned short`     | integer            | 2             | (3)      |
| `i`    | `int`                | integer            | 4             | (3)      |
| `I`    | `unsigned int`       | integer            | 4             | (3)      |
| `l`    | `long`               | integer            | 4             | (3)      |
| `L`    | `unsigned long`      | integer            | 4             | (3)      |
| `q`    | `long long`          | integer            | 8             | (2), (3) |
| `Q`    | `unsigned long long` | integer            | 8             | (2), (3) |
| `f`    | `float`              | float              | 4             | (4)      |
| `d`    | `double`             | float              | 8             | (4)      |
| `s`    | `char[]`             | string             |               |          |
| `p`    | `char[]`             | string             |               |          |
| `P`    | `void *`             | integer            |               | (5), (3) |

**Example**

```python
printer_control = struct.pack("<13B",
  0x1d, 0x73, 0x03, 0xe8, # max printer speed
  0x1d, 0x61, 0xd0,       # printer acceleration
  0x1d, 0x2f, 0x0f,       # peak current
  0x1d, 0x44, 0x80,       # max intensity
)
```

Little Endian encoded 13 unsigned chars
