# Tkinter

If you get abnormal fonts, check pyenv is set up correctly.

## Minimal app

```python
import tkinter as tk
from tkinter import ttk


def main() -> None:
    root = tk.Tk()
    root.title('Hello world')
    root.geometry('400x400')
    root.mainloop()


if __name__ == '__main__':
    main()

```
