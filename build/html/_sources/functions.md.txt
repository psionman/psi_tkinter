# Functions

[Trace a variable](#trace-a-variable)

[Set focus](#set-focus)

[File picker](#file-picker)

[Directory picker](#directory-picker)

[Default font](#default-font)

## Trace a variable

```python

        self.<variable>.trace_add('write', self._data_changed)
```

```python

    def _data_changed(self, *args) -> None:
        self.button_frame.enable(False)
        if (
            self.name.get() != self.project.name
            or ...
        ):
            self.button_frame.enable(True)
```

NB Use PsiText widget to detect changes in a tk.Text widget and bind it to the '\<\<TextModified>>' command

```python

        self.notes_text = PsiText(frame, height=18)
        ...
        self.notes_text.bind('<<TextModified>>', self._notes_changed)
```

## Set focus

```python

        text.focus_set()
```

## File picker

```python
TXT_FILE_TYPES = (
    ('text files', '*.txt'),
    ('All files', '*.*')
)
```

```python
from tkinter import filedialog
```

```python
    def _get_data_file_(self) -> str:
        """Return a path to a file."""
        path = filedialog.askopenfilename(
            title='Data file',
            initialfile=self.data_file.get(),
            parent=self.root,
            filetypes=TXT_FILE_TYPES
        )
        if not path:
            return
        self.data_file.set(path)
        return path
```

## Directory picker

```python
from tkinter import filedialog
```

```python
    def _get_data_directory(self) -> str:
        """Return a directory."""
        directory = filedialog.askdirectory(
            title='Data Directory',
            initialdir=self.data_directory.get(),
            parent=self.root,
        )
        if not directory:
            return
        self.data_directory.set(directory)
        return directory
```

## Default font

```python

        font_size = font.nametofont('TkTextFont').actual()['size']
```
