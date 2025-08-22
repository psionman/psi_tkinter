# Menus

## Menu

```python
    from psiutils.menus import Menu, MenuItem
```

```python
import tkinter as tk
from tkinter import messagebox
import webbrowser

from psiutils.menus import Menu, MenuItem

from constants import AUTHOR, APP_TITLE, HELP_URI
from version import __version__
import text
from config import config

from forms.frm_config import ConfigFrame

SPACES = ' '*20


class MainMenu():
    def __init__(self, parent):
        self.parent = parent
        self.root = parent.root

    def create(self):
        menubar = tk.Menu()
        self.root['menu'] = menubar

        # File menu
        file_menu = Menu(menubar, self._file_menu_items())
        menubar.add_cascade(menu=file_menu, label='File')

        # Help menu
        help_menu = Menu(menubar, self._help_menu_items())
        menubar.add_cascade(menu=help_menu, label='Help')

    def _file_menu_items(self) -> list:
        return [
            MenuItem(f'{text.CONFIG}{text.ELLIPSIS}', self._show_config_frame),
            MenuItem(text.EXIT, self.dismiss),
        ]

    def _show_config_frame(self):
        """Display the config frame."""
        dlg = ConfigFrame(self)
        self.root.wait_window(dlg.root)

    def _help_menu_items(self) -> list:
        return [
            MenuItem(f'On line help{text.ELLIPSIS}', self._show_help),
            MenuItem(f'Data directory location{text.ELLIPSIS}',
                     self._show_data_directory),
            MenuItem(f'About{text.ELLIPSIS}', self._show_about),
        ]

    def _show_help(self):
        webbrowser.open(HELP_URI)

    def _show_data_directory(self):
        dir = f'Data directory: {config.data_directory} {SPACES}'
        messagebox.showinfo(title='Data directory', message=dir)

    def _show_about(self):
        about = (f'{APP_TITLE}\n'
                 f'Version: {__version__}\n'
                 f'Author: {AUTHOR} {SPACES}')
        messagebox.showinfo(title=f'About {APP_TITLE}', message=about)

    def dismiss(self, args):
        self.root.destroy()
```

## Context menu

```python
from psiutils.menus import Menu, MenuItem
```

```python
    self.context_menu = self._context_menu()
```

```python

    def _show_context_menu(self, event) -> None:
        self.context_menu.tk_popup(event.x_root, event.y_root)

        ... # either

        selected_item = self.tree.identify_row(event.y)
        self.tree.selection_set(selected_item)

        ... # or

        self.listbox.selection_clear(0, tk.END)
        self.listbox.selection_set(self.listbox.nearest(event.y))
        self._project_selected(event)

    def _context_menu(self) -> tk.Menu:
        menu_items = [
            MenuItem('text', <function>, dimmable=True),
            ...
        ]
        context_menu = Menu(self.root, menu_items)
        context_menu.enable(False)
        return context_menu
```
