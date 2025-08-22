# Snippets

[Clear a frame](#clear-a-frame) [Call a modal frame](#call-a-modal-frame) [Frame got focus](#frame-got-focus) [Disable a frame](#disable-frame)

[Pass parameter to a bound event](#pass-parameter-to-a-bound-event)

[Set Listbox selection](#set-listbox-selection) [Enable buttons](#enable-buttons) [Label font](#label-font) [Global font](#global-font)

[Widget size](#widget-size)

[Centre an element](#centre-an-element)

[Wait cursor](#wait-cursor) [Update idle tasks](#update-idle-tasks)

[Style maps](#style-maps)

## Clear a frame

```python

        for widget in self.thumbnails.winfo_children():
            widget.destroy()
```

## Call a modal frame

```python

        dlg = xxxFrame(self)
        self.root.wait_window(dlg.root)
```

## Disable frame

```python

        for child in self.xxx_frame.winfo_children():
            child.configure(state='disable')
```

## Pass parameter to a bound event

```python

        button.bind('<Button-1>', lambda event, button=save_button:
                     self._enable_button(button))
```

## Set Listbox selection

```python

        selection = self.listbox.curselection()
        self.listbox.select_clear(selection[0])
        index = <list>.index(<item>)
        self.listbox.select_set(index)
        self.listbox.event_generate("<<ListboxSelect>>")
```

## Enable buttons

```python

        enable = False
        if <condition>:
            enable = True
        enable_buttons(self.buttons, enable)
```

## Widget size

```python
ic(self.frame.winfo_width())
ic(self.frame.winfo_height())
```

## Centre an element

```python
    frame = ttk.Frame(container)
    frame.rowconfigure(0, weight=1)
    frame.columnconfigure(0, weight=1)

    label = ttk.Label(frame, text='abc')
    label.grid(row=0, column=0)
```

## Label font

```python
    header = ttk.Label(frame, text=text, font=('Arial', 16))
```

## Styles

NB use with ttk widgets

```python
def styles():
    style = ttk.Style()
    style.configure("red.TLabel", foreground="red")
    style.configure('blue.TFrame', background='blue')
    style.configure('red.TFrame', background='red')
    style.configure('yellow.TFrame', background='yellow')
```

```python
    label = ttk.Label(frame, text='xyz', style='red.TLabel',
```

## Global font

```python
    style.configure('.', font=('Helvetica', 12))
```

## Wait cursor

```python

from psiutils.widgets import WaitCursor
```

```python

        with WaitCursor(self.root):
            <some process>
```

## Update idle tasks

```python
        self.root.update_idletasks() # Refresh UI without full redraw
```


## Directory selector

```python

    def _get_directory(self, *args) -> str:
        directory = filedialog.askdirectory(
            initialdir=self.directory.get(),
            parent=self.root,
        )
        if directory:
            self.directory.set(directory)
        return directory
```

## File picker

```python
    ...
        row = 0
        label = ttk.Label(frame, text='Membership file')
        label.grid(row=row, column=0, sticky=tk.E, padx=PAD, pady=PAD)

        entry = ttk.Entry(frame, textvariable=self.member_file)
        entry.grid(row=row, column=1, sticky=tk.EW)

        button = IconButton(frame, text.OPEN, 'open', self._get_member_file)
        button.grid(row=row, column=2, padx=PAD)
```

```python
    def _get_member_file(self, *args) -> None:
        member_file = filedialog.askopenfilename(
            initialdir=DOWNLOADS_DIR,
            initialfile=self.member_file.get(),
            filetypes=CSV_FILE_TYPES,
        )
        if member_file:
            self.member_file.set(member_file)
```

## Frame got focus

```python

        self.root.bind("<FocusIn>", self._got_focus)
```

## Drag a frame

```python

        root.bind('<Button-1>', self._click_on_window)
        root.bind('<B1-Motion>', self._drag_window)
```

```python

    def _drag_window(self, *args):
        root = self.root
        x = self.window_x_pos + root.winfo_pointerx() - self.start_x
        y = self.window_y_pos + root.winfo_pointery() - self.start_y
        root.geometry(f'+{x}+{y}')

    def _click_on_window(self, event):
        self.window_x_pos = self.root.winfo_rootx()
        self.window_y_pos = self.root.winfo_rooty()

        self.start_x = self.root.winfo_pointerx()
        self.start_y = self.root.winfo_pointery()

```

## Style maps

See [documentation](https://docs.python.org/3/library/tkinter.ttk.html)


```python

        self.style.configure(
            'green.TCheckbutton',
            foreground='white',
            background='green',
            )
        self.style.map(
            "green.TCheckbutton",
            foreground=[('pressed', 'red'), ('active', 'green')],
            background=[('pressed', '!disabled', 'black'), ('active', 'white')]
            )
```
