# Treeview

```python
from psiutils.treeview import sort_treeview
```

```python

TREE_COLUMNS = (
    ('<key>', 'Text', <width>),
)
```

```python

        self.tree = self._get_tree(frame)
        self.tree.grid(row=0, column=0, sticky=tk.NSEW)
        self._populate_tree()
```

```python

    def _get_tree(self, master: tk.Frame) -> ttk.Treeview:
        """Return  a tree widget."""
        tree = ttk.Treeview(
            master,
            selectmode='browse',
            height=15,
            show='headings',
            )
        tree.bind('<<TreeviewSelect>>', self._tree_clicked)
        tree.bind('<Button-3>', self._show_context_menu)

        col_list = tuple(col[0] for col in TREE_COLUMNS)
        tree['columns'] = col_list
        for col in TREE_COLUMNS:
            (col_key, col_text, col_width) = (col[0], col[1], col[2])
            tree.heading(col_key, text=col_text,
                         command=lambda c=col_key:
                         sort_treeview(tree, c, False))
            tree.column(col_key, width=col_width, anchor=tk.W)
        # tree.column(<'right-align-column-name'>, stretch=0, anchor=tk.E)
        #tree.configure(yscrollcommand=v_scroll.set)
        return tree
```

```python

    def _populate_tree(self) -> None:
        self.tree.delete(*self.tree.get_children())
        for key, item in <dict>.items():
            values = (key, item.xxx)
            self.tree.insert('', 'end', values=values)
```

## Treeview clicked

```python

    def _tree_clicked(self, *args) -> None:
        self.selected_item = self.tree.selection()
        values = self.tree.item(self.selected_item)['values']
```

## Context menu

```python

        self.context_menu = self._context_menu()
```

```python

    def _show_context_menu(self, event) -> None:
        self.context_menu.tk_popup(event.x_root, event.y_root)
        selected_item = self.tree.identify_row(event.y)
        self.tree.selection_set(selected_item)

    def _context_menu(self) -> tk.Menu:
        menu_items = [
            MenuItem('text', <function>, dimmable=True),
            ...
        ]
        context_menu = Menu(self.root, menu_items)
        context_menu.enable(False)
        return context_menu
```

## Get data from selected item

```python

        item: tuple = self.tree.item(self.selected_item, 'values')
```

## Update selected item

```python

            values = (<value-1>, ...)
            self.tree.item(self.selected_item, values=values)
```

## Add item and selected

```python

            values = (<value-1>, ...)
            item = self.tree.insert('', 'end', values=values)
            self.tree.selection_set(item)
```

## Clear Treeview

```python

self.tree.delete(*self.tree.get_children())
```

## Save selected item and restore after populate

The principle is to store the key of the selected item and to set the selection on populate tree

```
    def _tree_clicked(self, *args) -> None:
        selected_item = self.tree.selection()
        if not selected_item:
            return
        values = self.tree.item(selected_item)['values']
        self.selected_object = values[0]
```

```python
    def _populate_tree(self) -> None:
        for key, car in cars.items():
            values = [key] + [value for value in car]
            item = self.tree.insert('', 'end', values=values)
            if self.selected_object and key == self.selected_object:
                self.tree.selection_set(item
```

##  Colour rows

```python
    def _populate_tree(self, *args) -> None:
        self.tree.delete(*self.tree.get_children())
        style = ttk.Style()
        style.map(
            'Treeview',
            background=self._fixed_map(style, 'background'))
        self.tree.tag_configure('mismatch', background='pink')
        self.tree.tag_configure('match', background='white')

        comparison = <some divct>
        for key, item in comparison.items():
            if not item['match'] or self.show_mismatches.get():
                tag = 'match'
                if not item['match']:
                    tag = 'mismatch'
                values = <iterator from dict>
                self.tree.insert('', 'end', values=values, tags=(tag,))

    @staticmethod
    def _fixed_map(style, option):
        return [elm for elm in style.map("Treeview", query_opt=option)
                if elm[:2] != ("!disabled", "!selected")]
```

## Vertical scroll bar

```python

from psiutils.widgets import vertical_scroll_bar
```

```python

        v_scroll = vertical_scroll_bar(frame, self.tree)
        v_scroll.grid(row=1, column=1, sticky=tk.NS)
```

```python

        # root.bind("<Button-4>", self._on_mouse_wheel)
        # root.bind("<Button-5>", self._on_mouse_wheel)

    def _on_mouse_wheel(self, event):
        if event.num == 4:   # Linux scroll up
            self.tree.yview_scroll(-1, "units")
        elif event.num == 5:  # Linux scroll down
            self.tree.yview_scroll(1, "units")
        else:                # Windows / macOS
            self.tree.yview_scroll(-1 * (event.delta // 120), "units")
```
