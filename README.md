docstring_parser
================

Parse Python docstrings. Currently support ReST-style and Google-style
docstrings.

Example usage:

```python
>>> from docstring_parser import parse
>>>
>>>
>>> docstring = parse(
...     '''
...     Short description
...
...     Long description spanning multiple lines
...     - First line
...     - Second line
...     - Third line
...
...     :param name: description 1
...     :param int priority: description 2
...     :param str sender: description 3
...     :raises ValueError: if name is invalid
...     ''')
>>>
>>> docstring.long_description
'Long description spanning multiple lines\n- First line\n- Second line\n- Third line'
>>> docstring.params[1].arg_name
'priority'
>>> docstring.raises[0].type_name
'ValueError'
```

Is also possible to parse Google docstrings with custom fields.

```python
from docstring_parser.google import GoogleParser, Section, SectionType

parser = GoogleParser()
parser.add_section(Section("Note", "note", SectionType.SINGULAR))

docstring = parser.parse(
    """
    short description
        
    Note:
        a note
    """
)
assert len(docstring.meta) == 1
assert docstring.meta[0].args == ["note"]
assert docstring.meta[0].description == "a note"
```
