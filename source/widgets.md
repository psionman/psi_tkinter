# Widgets

[Basic Entry](#basic-entry) |
[Entry with label](#entry-with-label) |
[Enhanced Entry](#enhanced-entry) |

[Label](#label)

[Button](#button)

[Button frame](frames.html#button-frame)

[Text area](#text-area)

[Combobox](#combobox)

[Spinbox](#spinbox)

[Listbox](#listbox)

[Checkbutton](#checkbutton)

[Radiobutton](#radiobutton)

[Treeview](treeview.html#treeview)

[Images](#images)

[Tooltip](#tooltip)

[HtmlFrame](#htmlframe)

[Scrolling canvas](#scrolling-canvas)

## Colours

```bash
python ~/projects/utilities/colours/src/main.py
```

## Basic Entry

```python

        entry = ttk.Entry(frame, textvariable=self.variable)
        entry.grid(row=0, column=1, sticky=tk.EW)
```

## Entry with label

```python

        label = ttk.Label(frame, text='text')
        label.grid(row=0, column=0, sticky=tk.E, padx=PAD, pady=PAD)

        entry = ttk.Entry(frame, textvariable=self.variable)
        entry.grid(row=0, column=1, sticky=tk.EW)
```

## Enhanced Entry

```python

        state = 'normal'
        if self.mode == MODES['delete']:
            state = 'readonly'

        entry = ttk.Entry(frame, textvariable=self.variable, state=state)
        entry.grid(row=0, column=1, sticky=tk.EW)
        entry.bind('<KeyRelease>', self.function)
        entry.focus_set()
        entry.select_range(start=0, end='end')
        entry.icursor(len(variable.get()))
```

### Background colour

```python

        entry_style = ttk.Style()
        entry_style.configure(
            'my_style.TEntry',
            fieldbackground='aqua',
            )
        entry.configure(style='my_style.TEntry')
```

or

```python
        entry['style'] = 'my_style.TEntry'
```

### Password entry

```python
        self.password_entry = ttk.Entry(
            frame, textvariable=self.password, show='*')
```

### Trace a variable

```python
self.variable.trace_add('write', self._value_changed)
```

```python

    def _value_changed(self, *args):
        if (self.xxx != self.xxx_value
                or self.yyy != self.yyy_value
                ...):
        self.button_frame.enable()
```

### Disable focus

```python
takefocus=0
```

[top](#widgets)

## Label

```python

        label = ttk.Label(frame, text='xxx')
        label.grid(row=0, column=0)
        label.config(font=('TkDefaultFont', 44, 'bold'))
        label.bind('<Button-1>', self._selected)
```

```python

    label = ttk.Label(root, text='ttk-centered', width=25,
                      relief=tk.SUNKEN, anchor=tk.CENTER)
```

### Sunken label

NB *borderwidth=1* makes label look like a *readonly* entry

```python

        label = ttk.Label(frame, text='xxx', borderwidth=1, relief=tk.SUNKEN)
```

[top](#widgets)

## Button

```python

        button = ttk.Button(frame, text='Exit', command=self.dismiss,
                            underline=1)
```

To disable

```python

        button.state(['disabled'])
```

To enable

```python

        button.state(['!disabled'])
```

### Buttons in a loop

```python

        for index in range(ANSWERS):
            button = ttk.Button(
                frame,
                text=index+1,
                command=lambda index=index: self._check_answer(index))
```

[top](#widgets)


## Text area

```python

        text = tk.Text(frame, height=20)
        text.grid(row=0, column=0, sticky=tk.NSEW)
        text.insert('0.0', '\n'.join(<text>))
```

To make readonly

```python

        text.bind('<Key>', lambda e: 'break')
```

To retrieve the text contents

```python

        text = self.text.get('0.0', tk.END)
```

to clear the text

```python

        self.text.delete('0.0', tk.END)
```

[top](#widgets)

## Combobox

```python

        combobox = ttk.Combobox(
            frame,
            textvariable=self.variable,
            values=[x for x in values],
            )
        combobox.grid(row=0, column=1, sticky=tk.W)
        clickable_widget(combobox)
```

[top](#widgets)

## Spinbox

```python

        label = ttk.Label(frame, text='<text>')
        label.grid(row=0, column=4, sticky=tk.E)

        spinbox = ttk.Spinbox(
            frame,
            format='%.2f',
            from_=0.5,
            to=100,
            increment=0.5,
            textvariable=None)
        spinbox.grid(row=0, column=5, sticky=tk.E, padx=PAD)
        clickable_widget(spinbox)
```
To detect change add a trace to textvariable

[top](#widgets)

## Listbox

```python

        self.langs = ('Java', 'C#', 'C', 'C++', 'Python',
         'Go', 'JavaScript', 'PHP', 'Swift')

        self.langs_list = tk.Variable(value=langs)
        # langs_list.get() is str(langs)

        ...

        listbox = tk.Listbox(
            frame,
            listvariable=self.langs_list,
            height=6,
            selectmode=tk.BROWSE,
            cursor=HAND,
        )
        listbox.grid(row=1, column=1, sticky=tk.E, padx=PAD)
        listbox.bind('<<ListboxSelect>>', self._partner_selected)

    def _partner_selected(self, *args) -> None:
        selection = event.widget.curselection()
        if not selection:
            return
        lang = self.self.langs[selection[0]]
```

To set the selection

```python

        listbox.select_set(index)
```

To colour specific rows

```python

        for chat_text in self.chat:
            self.chat_listbox.insert('end', chat_text)
            if '---' in chat_text:
                self.chat_listbox.itemconfig('end', fg='red')
```

[top](#widgets)

## Checkbutton

```python

        check_button = ttk.Checkbutton(frame, text='text',
                                      variable=variable)
        check_button.grid(row=0, column=1, sticky=tk.W)
```

[top](#widgets)

## Radiobutton

*text* appears to the right of the button

*variable* a tk variable that links the radiobuttons into a group

*value* is the value placed in the *variable* when the button is clicked

NB use the *command* attribute to invoke an event on click

e.g.

```python

        for index, player in enumerate(self.players):
            button = ttk.Radiobutton(
                frame,
                text=player,
                variable=self.player,
                value=player,
            )
```

### Simple

```python

    button = ttk.Radiobutton(
        frame,
        text='text',
        variable=variable,
        value=value,
    )
    button.grid(row=1, column=0, sticky=tk.W)
```

### Enhanced

```python

    button = ttk.Radiobutton(
        frame,
        text='text',
        variable=variable,
        value=value,
        command=function,
    )
    if condition:
        button.invoke()
    button.grid(row=1, column=0, sticky=tk.W)
```

[top](#widgets)

## Separator frame

```python

        separator = self._separator_frame(frame, 'text')
        separator.grid(row=0, column=0, columnspan=3,
                       sticky=tk.EW, padx=PAD, pady=PAD)
```

```python

    def _separator_frame(self, master: tk.Frame, text: str) -> tk.Frame:
        frame = ttk.Frame(master)
        frame.columnconfigure(0, weight=1)
        frame.columnconfigure(2, weight=1)

        separator = ttk.Separator(frame, orient='horizontal')
        separator.grid(row=0, column=0, sticky=tk.EW,
                       padx=PAD, pady=PAD*4)

        label = ttk.Label(frame, text=text)
        label.grid(row=0, column=1, sticky=tk.E)

        separator = ttk.Separator(frame, orient='horizontal')
        separator.grid(row=0, column=2, sticky=tk.EW, padx=PAD)
        return frame
```

[top](#widgets)

## Images

```python

from PIL import ImageTk, Image
```

```python

        for column, file in enumerate(files):
            img = Image.open(file)
            scale = 100 / img.height
            img = img.resize(
                (int(img.width * scale), int(img.height * scale)),
                Image.LANCZOS)
            img = ImageTk.PhotoImage(img)
            panel = ttk.Label(self.thumbnails, image=img)
            panel.image = img
            panel.grid(row=0, column=column)
```

[top](#widgets)

## Tooltip

```python

from psiutils.widgets import Tooltip
```

```python

        entry.tooltip = Tooltip(entry, textvariable=entry.tooltip_text)
        entry.bind('<Enter>', entry.tooltip.onEnter)
        entry.bind('<Leave>', entry.tooltip.onLeave)
```

[top](#widgets)

## HtmlFrame

**NB Throws an error if you have a module named *utilities.py* in your project**

```python
from tkinterweb import HtmlFrame
```

```python

        self.html_frame = HtmlFrame(
            frame, horizontal_scrollbar='auto', messages_enabled=False)
        self.html_frame.grid(row=0, column=0, sticky=tk.NSEW)
```

```python

        html = display_html(self.html_frame, output, self.config.css)
```

```python



def display_html(html_frame: HtmlFrame, text: str, css: dict = None) -> None:
    if not css:
        css = {}
    css_str = _css_from_dict(css)
    html = markdown.markdown(text)
    page = f"""
        <!DOCTYPE html>
            <html lang="en">
                <head>
                    <meta charset="utf-8">
                    <style>{css_str}</style>
                </head>
                <body>
                    <h1>{html}</h1>
                </body>
            </html>"""

    _write_html_file(html_frame, 'dummy.html', '')
    _write_html_file(html_frame, 'temp.html', page)
    return page


def _write_html_file(html_frame: HtmlFrame, file: str, text: str) -> None:
    temp_path = Path(USER_DATA_DIR, file)
    path = str(temp_path)
    with open(path, 'w') as f_html:
        f_html.write(text)
    html_frame.load_file(path)


def _css_from_dict(css: dict) -> str:
    css_str = ''
    for element, item in css.items():
        css_str = f'{css_str}{element}{{'
        for attribute, value in item.items():
            if attribute == 'font-size':
                value = f'{value}px'
            css_str = f'{css_str}{attribute}:{value};'
        css_str = f'{css_str}}}'
    return css_str

```

## Scrolling canvas

```python
    def _main_frame(self, master: tk.Frame) -> ttk.Frame:
        frame = ttk.Frame(master)
        frame.rowconfigure(0, weight=1)
        frame.columnconfigure(0, weight=1)

        # Create the canvas
        self.canvas = tk.Canvas(frame, borderwidth=0)
        self.canvas.grid(row=0, column=0, sticky=tk.NSEW)

        # Create a frame that will fit on the canvas and hold
        # the items to be scolled
        slave_frame = self._canvas_frame(self.canvas)

        # The scroll bar
        v_scroll = tk.Scrollbar(
            self, orient=tk.VERTICAL, command=self.canvas.yview)
        v_scroll.grid(row=0, column=1, sticky=tk.NS)

        self.canvas.configure(yscrollcommand=v_scroll.set)
        self.canvas.create_window((4, 4), window=slave_frame, anchor=tk.NW)

        self._populate(slave_frame) # e.g. a list of buttons or labels
        return frame

    def _canvas_frame(self, master) -> tk.Frame:
        frame = tk.Frame(master)
        frame.bind("<Configure>", self._frame_configure)
        return frame

    def _frame_configure(self, *args):
        """Reset the scroll region to encompass the inner frame"""
        self.canvas.configure(scrollregion=self.canvas.bbox('all'))
```
