# Screens and Frames

[Root](#root) |
[Main Screen](#main-screen) |
[Modal Screen](#modal-screen) |
[Config Maintenance Screen](#config-maintenance-screen) |
[Main frame](#main-frame) |
[Button frame](#button-frame) |
[Menu](#menu) |
[Add, Delete, Update](#add-delete-update) |
[Edit frame](#edit-frame) |
[Geometry](#geometry)

## Root

```python

"""A tkinter application for <application>."""

import tkinter as tk

from psiutils.widgets import get_styles
from psiutils.utilities import display_icon

from constants import (ICON_FILE)

from forms.frm_main import MainFrame


class Root():
    def __init__(self) -> None:
        """Create the app's root and loop."""
        self.root = tk.Tk()
        root = self.root
        root.option_add('*tearOff', False)
        display_icon(root, ICON_FILE, ignore_error=True)
        root.protocol("WM_DELETE_WINDOW", root.destroy)

        get_styles()

        MainFrame(self)
        root.mainloop()
```

[top](#screens-and-frames)

## Main Screen

```python


"""MainFrame for <application>."""
import tkinter as tk
from tkinter import ttk
from pathlib import Path

from psiutils.constants import PAD
from psiutils.buttons import ButtonFrame, Button
from psiutils.utilities import window_resize

from constants import APP_TITLE
from config import read_config
import text

from main_menu import MainMenu

FRAME_TITLE = APP_TITLE


class MainFrame():
    def __init__(self, parent) -> None:
        self.root = parent.root
        self.config = read_config()

        # tk variables

        self.show()

    def show(self):
        root = self.root
        root.geometry(self.config.geometry[Path(__file__).stem])
        root.title(FRAME_TITLE)

        root.bind('<Control-x>', self.dismiss)
        root.bind('<Control-o>', self._process)
        root.bind('<Configure>',
                  lambda event, arg=None: window_resize(self, __file__))

        main_menu = MainMenu(self)
        main_menu.create()

        root.rowconfigure(0, weight=1)
        root.columnconfigure(0, weight=1)

        main_frame = self._main_frame(root)
        main_frame.grid(row=0, column=0, sticky=tk.NSEW, padx=PAD, pady=PAD)

        self.button_frame = self._button_frame(root)
        self.button_frame.grid(row=8, column=0, columnspan=9,
                               sticky=tk.EW, padx=PAD, pady=PAD)

        sizegrip = ttk.Sizegrip(root)
        sizegrip.grid(sticky=tk.SE)

    def _main_frame(self, master: tk.Frame) -> ttk.Frame:
        frame = ttk.Frame(master)
        # frame.rowconfigure(0, weight=1)
        # frame.columnconfigure(1, weight=1)

        return frame

    def _button_frame(self, master: tk.Frame) -> tk.Frame:
        frame = ButtonFrame(master, tk.HORIZONTAL)
        frame.buttons = [
            Button(
                frame,
                text=text.OK,
                command=self._process,
                underline=0,
                dimmable=True),
            Button(
                frame,
                text=text.EXIT,
                command=self.dismiss,
                sticky=tk.E,
                underline=1),
        ]
        frame.enable(False)
        return frame

    def _process(self, *args) -> None:
        ...

    def dismiss(self, *args) -> None:
        self.root.destroy()

```

### Call a modal frame

```python

        dlg = xxxFrame(self)
        self.root.wait_window(dlg.root)
```

[top](#screens-and-frames)

## Modal Screen

```python
"""xxxFrame for <application>."""
import tkinter as tk
from tkinter import ttk
from pathlib import Path

from psiutils.constants import PAD
from psiutils.buttons import ButtonFrame, Button
from psiutils.utilities import window_resize

from constants import APP_TITLE, DEFAULT_GEOMETRY
from config import read_config
import text

FRAME_TITLE = f'{APP_TITLE} - <title>'

class xxxFrame():
    def __init__(self, parent: tk.Frame) -> None:
        self.root = tk.Toplevel()
        self.parent = parent
        self.config = read_config()

        # tk variables

        self.show()

    def show(self) -> None:
        root = self.root
        try:
            root.geometry(self.config.geometry[Path(__file__).stem])
        except KeyError:
            root.geometry(DEFAULT_GEOMETRY)
        root.title(FRAME_TITLE)
        root.transient(self.parent.root)

        # Make modal
        root.grab_set()

        root.bind('<Configure>',
                  lambda event, arg=None: window_resize(self, __file__))

        root.bind('<Control-x>', self.dismiss)

        root.rowconfigure(0, weight=1)
        root.columnconfigure(0, weight=1)

        main_frame = self._main_frame(root)
        main_frame.grid(row=0, column=0, sticky=tk.NSEW, padx=PAD, pady=PAD)
        self.button_frame = self._button_frame(root)
        self.button_frame.grid(row=8, column=0, columnspan=9,
                               sticky=tk.EW, padx=PAD, pady=PAD)

        sizegrip = ttk.Sizegrip(root)
        sizegrip.grid(sticky=tk.SE)

    def _main_frame(self, master: tk.Frame) -> ttk.Frame:
        frame = ttk.Frame(master)
        # frame.rowconfigure(0, weight=1)
        frame.columnconfigure(1, weight=1)

        return frame

    def _button_frame(self, master: tk.Frame) -> tk.Frame:
        frame = ButtonFrame(master, tk.HORIZONTAL)
        frame.buttons = [
            Button(
                frame,
                text=text.OK,
                command=self._process,
                underline=0,
                dimmable=True),
            Button(
                frame,
                text=text.EXIT,
                command=self.dismiss,
                sticky=tk.E,
                underline=1),
        ]
        frame.enable(False)
        return frame

    def _process(self, *args) -> None:
        ...

    def dismiss(self, *args) -> None:
        self.root.destroy()

```

[top](#screens-and-frames)

## Config Maintenance Screen

```python

"""ConfigFrame for <application>."""

import tkinter as tk
from tkinter import ttk
from pathlib import Path

from psiutils.buttons import ButtonFrame, Button
from psiutils.constants import PAD
from psiutils.utilities import window_resize

from constants import APP_TITLE
from config import read_config, save_config
import text


class ConfigFrame():
    def __init__(self, parent: tk.Frame) -> None:
        self.root = tk.Toplevel(parent.root)
        self.parent = parent
        self.config = read_config()

        # tk variables
        self.xxx = tk.StringVar(value='')

        self.xxx.trace_add('write', self._check_value_changed)

        self.show()

    def show(self) -> None:
        root = self.root
        root.geometry(self.config.geometry[Path(__file__).stem])
        root.transient(self.parent.root)
        root.title(f'{APP_TITLE} - {text.CONFIG}')

        root.bind('<Control-x>', self.dismiss)
        root.bind('<Control-s>', self._save_config)
        root.bind('<Configure>',
                  lambda event, arg=None: window_resize(self, __file__))

        root.rowconfigure(1, weight=1)
        root.columnconfigure(0, weight=1)

        main_frame = self._main_frame(root)
        main_frame.grid(row=0, column=0, sticky=tk.NSEW, padx=PAD, pady=PAD)
        self.button_frame = self._button_frame(root)
        self.button_frame.grid(row=8, column=0, columnspan=9,
                               sticky=tk.EW, padx=PAD, pady=PAD)

        sizegrip = ttk.Sizegrip(root)
        sizegrip.grid(sticky=tk.SE)

    def _main_frame(self, master: tk.Frame) -> ttk.Frame:
        frame = ttk.Frame(master)
        frame.rowconfigure(0, weight=1)
        frame.columnconfigure(1, weight=1)

        return frame

    def _button_frame(self, master: tk.Frame) -> tk.Frame:
        frame = ButtonFrame(master, tk.HORIZONTAL)
        frame.buttons = [
            Button(
                frame,
                text=text.SAVE,
                command=self._save_config,
                underline=0,
                dimmable=True),
            Button(
                frame,
                text=text.EXIT,
                command=self.dismiss,
                sticky=tk.E,
                underline=1),
        ]
        frame.enable(False)
        return frame

    def _value_changed(self) -> bool:
        return (
            self.xxx.get() != self.config.xxx or
            ...
        )

    def _check_value_changed(self, *args) -> None:
        enable = bool(self._value_changed())
        self.button_frame.enable(enable)

    def _save_config(self, *args) -> None:
        # To generate assignments from tk-vars run script: assignment-invert
        self.config.update('xxx', self.xxx.get())
        save_config(self.config)
        self.dismiss()

    def dismiss(self, *args) -> None:
        self.root.destroy()

```

[top](#screens-and-frames)

## Main frame

```python

    def _main_frame(self, master: tk.Frame) -> ttk.Frame:
        frame = ttk.Frame(master)
        frame.rowconfigure(0, weight=1)
        frame.columnconfigure(0, weight=1)

        return frame
```

[top](#screens-and-frames)

## Button frame

```python

from psiutils.buttons import ButtonFrame, Button, HORIZONTAL, enable_buttons

import text
```

```python

    def _button_frame(self, master: tk.Frame) -> tk.Frame:
        buttons = [
            Button(text.OK, self._process, underline=0, dimmable=True),
            Button(text.EXIT, self.dismiss, tk.E, underline=1),
        ]
        frame = ButtonFrame(master, buttons, HORIZONTAL)
        frame.enable(False)
        return frame
```

[top](#screens-and-frames)

## Add, Delete, Update

```python


    def _new_project(self, *args) -> None:
        dlg = ProjectEditFrame(self, ps.NEW)
        self.root.wait_window(dlg.root)
        self._update_projects(dlg)

    def _edit_project(self, *args) -> None:
        dlg = ProjectEditFrame(self, ps.EDIT, self.project)
        self.root.wait_window(dlg.root)
        self._update_projects(dlg)

    def _delete_project(self, *args) -> None:
        dlg = messagebox.askyesno(
            'Delete project',
            f'Are you sure you want to delete {self.project.name}?',
            parent=self.root,
        )
        if dlg is False:
            return
        del self.projects[self.project.name]
        self._save_projects()

    def _update_projects(self, dlg: ttk.Frame) -> None:
        if dlg.status != ps.UPDATED:
            return
        self.projects[dlg.project.name] = dlg.project
        self._save_projects()

    def _save_projects(self) -> None:
        result = save_projects(self.projects)
        if result == ps.ERROR:
            messagebox.showerror(
                'Save',
                'Save failed',
                parent=self.root
            )
            return
        self._populate_tree()
```

[top](#screens-and-frames)

## Edit frame

```python

"""EditFrame for <item>s."""
import tkinter as tk
from tkinter import ttk, filedialog, messagebox

from psiutils.constants import PAD, Pad, MODES, DIALOG_STATUS
from psiutils.buttons import ButtonFrame, Button, HORIZONTAL
from psiutils.utilities import display_icon
from psiutils.widgets import clickable_widget

from constants import ICON_FILE
from data import ... save_<items>s
import text

GEOMETRY = '750x400'
FRAME_TITLE = 'Edit <item>'


class EditFrame():
    def __init__(self, parent: tk.Frame, <item>: <Item>, mode: int) -> None:
        self.root = tk.Toplevel(parent.root)
        self.parent = parent
        self.<item> = <item>
        self.<item>s = parent.<item>s
        self.mode = mode
        if mode == MODES['new']:
            self.<item> = <item>()

        # tk variables
        self.name = tk.StringVar(value=self.<item>.name)
        ...

        self.name.trace_add('write', self._data_changed)
        ...

        self.show()

    def show(self) -> None:
        root = self.root
        root.geometry(GEOMETRY)
        root.title(FRAME_TITLE)
        display_icon(root, ICON_FILE, ignore_error=True)
        root.wait_visibility()
        root.grab_set()
        root.transient(self.parent.root)
        root.bind('<Control-x>', self.dismiss)

        root.rowconfigure(0, weight=1)
        root.columnconfigure(0, weight=1)

        main_frame = self._main_frame(root)
        main_frame.grid(row=0, column=0, sticky=tk.NSEW, padx=PAD, pady=PAD)
        self.button_frame = self._button_frame(root)
        self.button_frame.grid(row=9, column=0, columnspan=9,
                               sticky=tk.EW, padx=PAD, pady=PAD)

        sizegrip = ttk.Sizegrip(root)
        sizegrip.grid(sticky=tk.SE)

    def _main_frame(self, master: tk.Frame) -> ttk.Frame:
        frame = ttk.Frame(master)
        # frame.rowconfigure(0, weight=1)
        frame.columnconfigure(1, weight=1)

        label = ttk.Label(frame, text='Name')
        label.grid(row=0, column=0, sticky=tk.E, padx=PAD, pady=PAD)

        state = 'readonly'
        if self.mode == MODES['new']:
            state = ''
        entry = ttk.Entry(frame, textvariable=self.name, state=state)
        entry.grid(row=0, column=1, sticky=tk.EW)

        return frame

    def _button_frame(self, master: tk.Frame) -> tk.Frame:
        buttons = [
            Button(text.SAVE, self._save, underline=0, dimmable=True),
            Button(text.EXIT, self.dismiss, tk.E, underline=1),
        ]
        frame = ButtonFrame(master, buttons, HORIZONTAL)
        frame.enable(False)
        return frame

    def _save(self, *args) -> None:
        self.<item>.name = self.name.get()
        ...
        response = save_<item>s(self.<item>s)
        if response == DIALOG_STATUS['ok']:
            self.dismiss()
            return
        messagebox.showerror(
            'Save',
            '<item>s not saved',
            parent=self.root
        )

    def _data_changed(self, *args) -> None:
        self.button_frame.enable(False)
        if (
            self.name.get() != self.<item>.name
            or ...
        ):
            self.button_frame.enable(True)

    def dismiss(self, *args) -> None:
        self.root.destroy()

```

[top](#screens-and-frames)

## Geometry

In *constants*

```python
GEOMETRY = {
    'frm_main': {
        'Linux': '500x600',
        'Windows': '500x600',
    },
    'frm_config': {
        'Linux': '700x300',
        'Windows': '700x300',
    },
}
```
