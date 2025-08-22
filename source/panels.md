# Panels

[Scrolling frame](#scrolling-frame)

[Paned window](#paned-window)

[Notebook widget](#notebook-widget)

## Scrolling frame

```python

from psiutils.widgets import VerticalScrolledFrame
```

```python

    def _main_frame(self, master: tk.Frame) -> ttk.Frame:
        frame = VerticalScrolledFrame(master)
        frame.grid(row=0, column=0, sticky=tk.NS)

        for row, foo in enumerate(bar):
            label = ttk.Label(frame.interior, text=foo)
            label.grid(row=row, column=1, sticky=tk.W)

        return frame
```

## Paned window

```python

    def _paned_window(self, master: ttk.Frame) -> ttk.PanedWindow:
        frame = ttk.PanedWindow(master, orient=tk.VERTICAL)

        listbox_one = tk.Listbox(frame)
        listbox_one.pack()
        frame.add(listbox_one)

        listbox_two = tk.Listbox(frame)
        listbox_two.pack()
        frame.add(listbox_two)

        return frame
```

## Notebook widget

```python

    def _get_notebook(self, master: tk.Frame) -> ttk.Notebook:
        notebook = ttk.Notebook(master)

        tab = ttk.Frame(notebook)
        notebook.add(tab, text='Tab 1')

        tab = ttk.Frame(notebook)
        notebook.add(tab, text='Tab 2')
```
